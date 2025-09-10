# 定义主版本和次版本
set(MAJOR_VERSION 1)
set(MINOR_VERSION 0)

# 处理构建号（PATCH_VERSION）
if(NOT EXISTS "${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt")
    # 首次构建：创建build_number.txt并初始化为0
    file(WRITE "${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt" "0")
endif()
# 读取当前构建号
file(READ "${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt" PATCH_VERSION)
# 构建号+1
math(EXPR PATCH_VERSION "${PATCH_VERSION} + 1")
# 写回新的构建号
file(WRITE "${CMAKE_CURRENT_SOURCE_DIR}/build_number.txt" "${PATCH_VERSION}")

# 用模板生成version.h（关键：确保这里的输入文件是version.h.in）
configure_file(version.h.in ${CMAKE_BINARY_DIR}/version.h @ONLY)

# 添加构建目录到头文件搜索路径，让编译器能找到生成的version.h
include_directories(${CMAKE_BINARY_DIR})