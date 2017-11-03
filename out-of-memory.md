Long-running `yarn test` in a `create-react-app` project which uses Jest. Command
was running for about a few days, press on `a` which causes all tests to rerun
failed with following error:

```
Determining test suites to run...
<--- Last few GCs --->

[1705:0x2e716e0] 31386347 ms: Scavenge 1160.0 (1428.0) -> 1159.3 (1428.0) MB, 8.3 / 0.0 ms  allocation failure
[1705:0x2e716e0] 31386440 ms: Scavenge 1160.0 (1428.0) -> 1159.1 (1428.0) MB, 20.4 / 0.0 ms  allocation failure
[1705:0x2e716e0] 31455723 ms: Scavenge 1160.1 (1428.0) -> 1159.2 (1428.0) MB, 11.9 / 0.0 ms  allocation failure
[1705:0x2e716e0] 31487688 ms: Scavenge 1160.2 (1428.0) -> 1159.2 (1428.0) MB, 5.8 / 0.0 ms  allocation failure


<--- JS stacktrace --->
Cannot get stack trace in GC.
FATAL ERROR: Scavenger: promoting marked
 Allocation failed - process out of memory
 1: node::Abort() [node]
 2: 0x12b82ac [node]
 3: v8::Utils::ReportOOMFailure(char const*, bool) [node]
 4: v8::internal::V8::FatalProcessOutOfMemory(char const*, bool) [node]
 5: 0xa98e0b [node]
 6: void v8::internal::ScavengingVisitor<(v8::internal::MarksHandling)0, (v8::internal::PromotionMode)0, (v8::internal::LoggingAndProfiling)1>::EvacuateObject<(v8::internal::ScavengingVisitor<(v8::internal::MarksHandling)0, (v8::internal::PromotionMode)0, (v8::internal::LoggingAndProfiling)1>::ObjectContents)0, (v8::internal::AllocationAlignment)0>(v8::internal::Map*, v8::internal::HeapObject**, v8::internal::HeapObject*, int) [node]
 7: v8::internal::Scavenger::ScavengeObject(v8::internal::HeapObject**, v8::internal::HeapObject*) [node]
 8: v8::internal::Heap::IteratePromotedObjectPointers(v8::internal::HeapObject*, unsigned char*, unsigned char*, bool, void (*)(v8::internal::HeapObject**, v8::internal::HeapObject*)) [node]
 9: void v8::internal::BodyDescriptorBase::IterateBodyImpl<v8::internal::ObjectVisitor>(v8::internal::HeapObject*, int, int, v8::internal::ObjectVisitor*) [node]
10: void v8::internal::BodyDescriptorApply<v8::internal::CallIterateBody, void, v8::internal::HeapObject*, int, v8::internal::ObjectVisitor*>(v8::internal::InstanceType, v8::internal::HeapObject*, int, v8::internal::ObjectVisitor*) [node]
11: v8::internal::Heap::DoScavenge(v8::internal::ObjectVisitor*, unsigned char*, v8::internal::PromotionMode) [node]
12: v8::internal::Heap::Scavenge() [node]
13: v8::internal::Heap::PerformGarbageCollection(v8::internal::GarbageCollector, v8::GCCallbackFlags) [node]
14: v8::internal::Heap::CollectGarbage(v8::internal::GarbageCollector, v8::internal::GarbageCollectionReason, char const*, v8::GCCallbackFlags) [node]
15: v8::internal::Factory::NewRawOneByteString(int, v8::internal::PretenureFlag) [node]
16: v8::internal::Factory::NewStringFromOneByte(v8::internal::Vector<unsigned char const>, v8::internal::PretenureFlag) [node]
17: v8::internal::Factory::NewStringFromUtf8(v8::internal::Vector<char const>, v8::internal::PretenureFlag) [node]
18: v8::String::NewFromUtf8(v8::Isolate*, char const*, v8::String::NewStringType, int) [node]
19: node::StringBytes::Encode(v8::Isolate*, char const*, unsigned long, node::encoding) [node]
20: void node::Buffer::StringSlice<(node::encoding)1>(v8::FunctionCallbackInfo<v8::Value> const&) [node]
21: v8::internal::FunctionCallbackArguments::Call(void (*)(v8::FunctionCallbackInfo<v8::Value> const&)) [node]
22: 0xb46f2c [node]
23: v8::internal::Builtin_HandleApiCall(int, v8::internal::Object**, v8::internal::Isolate*) [node]
24: 0xc758a5043a7
error Command failed with exit code 1.
```

# Increasing memory limit

See https://github.com/nodejs/node/issues/13018 which suggests a memory leak,
but in case limit should be increased, use  `node --max_old_space_size=2048`.
