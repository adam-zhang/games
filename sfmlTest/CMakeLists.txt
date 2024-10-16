cmake_minimum_required(VERSION 3.28)
# 设置 CMake 所需的最低版本为 3.28

project(CMakeSFMLProject LANGUAGES CXX)
# 创建一个名为 CMakeSFMLProject 的项目，使用 C++语言

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
# 设置可执行文件的输出目录为构建目录下的 bin 文件夹

option(BUILD_SHARED_LIBS "Build shared libraries" OFF)
# 设置一个选项，名为 BUILD_SHARED_LIBS，默认值为关闭（不构建共享库）

include(FetchContent)
# 引入 FetchContent 模块，用于获取外部项目

FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    # 声明要获取的外部项目 SFML，指定其 Git 仓库地址
    GIT_TAG 2.6.x
    # 指定要获取的 SFML 版本标签为 2.6.x
    GIT_SHALLOW ON
    # 开启浅克隆，以加快获取速度
    EXCLUDE_FROM_ALL
    # 该项目不包含在默认构建目标中
    SYSTEM)
# 声明获取 SFML 的相关信息

FetchContent_MakeAvailable(SFML)
# 使 SFML 项目可用，即完成获取和配置

add_executable(main src/main.cpp)
# 添加一个名为 main 的可执行目标，其源文件为 src/main.cpp

target_link_libraries(main PRIVATE sfml-graphics)
# 将 main 目标与 sfml-graphics 库链接

target_compile_features(main PRIVATE cxx_std_17)
# 为 main 目标设置编译特性为 C++17 标准

if(WIN32)
# 如果是 Windows 平台
    add_custom_command(
        TARGET main
        # 为 main 目标添加自定义命令
        COMMENT "Copy OpenAL DLL"
        # 注释说明该命令是复制 OpenAL DLL
        PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll $<TARGET_FILE_DIR:main>
        # 在构建 main 目标之前执行命令，复制 OpenAL DLL 文件到 main 目标的输出目录
        VERBATIM)
endif()
# 结束 Windows 平台的条件判断
