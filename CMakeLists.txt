file(GLOB SRC_FILES
    src/*.cpp
    )

add_llvm_pass_plugin(x86-isel
    ${SRC_FILES}
   )