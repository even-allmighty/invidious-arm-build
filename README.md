## invidious-arm-build
Attempt to build Invidious for arm (Raspberry PI)

# Current issues:

- Missing symbol errors when running invidious:arm
```
...
invidious_1  | Error relocating /invidious/invidious: _ZNSaIcED1Ev: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_document_end_event_initialize: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_emitter_delete: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm9DIBuilder21createUnspecifiedTypeENS_9StringRefE: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm5Value7setNameERKNS_5TwineE: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZNK4llvm5Value10getContextEv: symbol not found
invidious_1  | Error relocating /invidious/invidious: GC_set_push_other_roots: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_prepare_v2: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_create_function: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_mapping_end_event_initialize: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZdlPvj: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm9DIBuilder13insertDeclareEPNS_5ValueEPNS_15DILocalVariableEPNS_12DIExpressionEPKNS_10DILocationEPNS_10BasicBlockE: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm4UsernwEjjj: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_parser_set_input_string: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_bind_int64: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1ERKS4_: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm9DIBuilder15createArrayTypeEyjPNS_6DITypeENS_24MDTupleTypedArrayWrapperINS_6DINodeEEE: symbol not found
invidious_1  | Error relocating /invidious/invidious: GC_malloc_atomic: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm23ReplaceableMetadataImpl18replaceAllUsesWithEPNS_8MetadataE: symbol not found
invidious_1  | Error relocating /invidious/invidious: event_reinit: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_errcode: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_document_start_event_initialize: symbol not found
postgres_1   | 2021-06-11 18:59:49.110 UTC [1] LOG:  listening on Unix socket "/var/run/postgresql/.s.PGSQL.5432"
invidious_1  | Error relocating /invidious/invidious: sqlite3_column_count: symbol not found
invidious_1  | Error relocating /invidious/invidious: yaml_mapping_start_event_initialize: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_close: symbol not found
invidious_1  | Error relocating /invidious/invidious: LLVMInitializeMCJITCompilerOptions: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm21SymbolTableListTraitsINS_11InstructionEE13addNodeToListEPS1_: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm13EngineBuilder6createEPNS_13TargetMachineE: symbol not found
invidious_1  | Error relocating /invidious/invidious: GC_set_handle_fork: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZNSt7__cxx1112basic_stringIcSt11char_traitsIcESaIcEEC1EPKcRKS3_: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm9DIBuilder22createLexicalBlockFileEPNS_7DIScopeEPNS_6DIFileEj: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZNK4llvm8CallBase25hasFnAttrOnCalledFunctionENS_9Attribute8AttrKindE: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_column_type: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZNK4llvm8Function10getContextEv: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_errmsg: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm9DIBuilder18createLexicalBlockEPNS_7DIScopeEPNS_6DIFileEjj: symbol not found
invidious_1  | Error relocating /invidious/invidious: _ZN4llvm16MetadataTracking7retrackEPvRNS_8MetadataES1_: symbol not found
invidious_1  | Error relocating /invidious/invidious: sqlite3_bind_null: symbol not found
invidious_1  | Error relocating /invidious/invidious: GC_stackbottom: symbol not found
```
# Building:
## Build lsquic for arm
sudo docker buildx build -t invidious:lsquicBuilder --platform linux/arm/v7 -f Dockerfile_lsquicBuilder .

## Cross-compile but don't link invidious
sudo docker buildx build -t invidious:compile --platform linux/arm/v7 -f Dockerfile_compile .

## Link invidious on arm
sudo docker buildx build -t invidious:link --platform linux/arm/v7 -f Dockerfile_link .

## Build final image
sudo docker buildx build -t invidious:arm --platform linux/arm/v7 -f Dockerfile_invidiousARM .

## Export image for import on Raspberry PI
 sudo docker save invidious:arm | xz > invidious_arm.tar.xz