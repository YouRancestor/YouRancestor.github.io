# UEdGraphPin

蓝图图表节点引脚

## FEdGraphPinType

| 变量类型 | PinCategory | PinSubCategory | PinSubCategoryObject |
|-----|------|------|------|
| Boolean | "bool"(UEdGraphSchema_K2::PC_Boolean) | "None"(NAME_None) | nullptr |
| Integer | "int"(UEdGraphSchema_K2::PC_Int) | "None"(NAME_None) | nullptr |
| Integer64 | "int64"(UEdGraphSchema_K2::PC_Int64) | "None"(NAME_None) | nullptr |
| Byte | "byte"(UEdGraphSchema_K2::PC_Byte) | "None"(NAME_None) | nullptr |
| Float | "real"(UEdGraphSchema_K2::PC_Real) | "double"(UEdGraphSchema_K2::PC_Double) | nullptr |
| FSttring | "string"(UEdGraphSchema_K2::PC_String) | "None"(NAME_None) | nullptr |
| Vector | "struct"(UEdGraphSchema_K2::PC_Struct) | "None"(NAME_None) | TBaseStructure<FVector>::Get() |
| 蓝图类对象 | "ObjectTypes"(UEdGraphSchema_K2::AllObjectTypes) | "None"(NAME_None) | Blueprint->GeneratedClass |

### ContainerType

有容器（数组、集合、映射）时使用

- None(EPinContainerType::None)
- Array(EPinContainerType::Array)
- Set(EPinContainerType::Set)
- Map(EPinContainerType::Map)， PinCategory 存键类型，值类型存到 PinValueType 里
