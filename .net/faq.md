# FAQ for .NET SDK

### What's the impact on the code size?&#x20;

&#x20;Our .NET library only has a size of 24.2 MB.

### **I wrapped my function with LambdaRequestHandler, but I can't see any invocations. What’s wrong?**

Make sure your handler points to`<MyAssembly>::<MyNamespace>.<MyClass>::HandleRequest`✅

not `<MyAssembly>::<MyNamespace>.<MyClass>::DoHandleRequest`❌
