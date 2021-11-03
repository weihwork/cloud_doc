# get hands dirty

Why Rust

他选择 Rust 的理由：

因为 Rust 功能不如 Cpp 强大。 你没看错，他认为 Rust 不如 Cpp强大，所以选 Rust。他认为，当你给了代码编写者强大的力量，意味着，与此同时也剥夺了代码阅读者的权力。

Rust 语言优秀的地方就在于，让开发者在编写代码的过程中就去思考代码的安全性和健壮性。

struct 数据 -> protobuf 字节流 -> base64 编码-> url 请求

build.rs 可以在编译 cargo 项目时，做额外的编译处理。我们可以使用prost_build 把 abi.proto 编译到 src/pb 目录下，生产abi.rs文件，用作程序的数据结构。