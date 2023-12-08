cmake_minimum_required(VERSION 3.22)

project(latex-weekly)

set(PDFLATEX_COMPILER /Library/TeX/texbin/pdflatex)
set(CMAKE_INSTALL_NAME_DIR ${CMAKE_SOURCE_DIR}/out)

macro(compile_latex _meta_target _source)
    get_filename_component(_name ${_source} NAME_WE)
    set(_target ${_name}_compile)
    add_custom_target(${_target}
      COMMAND ${PDFLATEX_COMPILER} ${_source}
      WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR})

    install(FILES ${CMAKE_CURRENT_BINARY_DIR}/${_name}.pdf
            DESTINATION ${CMAKE_INSTALL_NAME_DIR})

    add_dependencies(${_meta_target} ${_target})
endmacro()

file(GLOB_RECURSE latex_sources *.tex)

set(meta_target latex_compile)
add_custom_target(${meta_target} ALL)
foreach(_source ${latex_sources})
    compile_latex(${meta_target} ${_source})
endforeach()