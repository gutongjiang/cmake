add_library(mymath mathfunctions.cxx)

install (TARGETS mymath DESTINATION bin)
install (FILES mathfunctions.h DESTINATION include)
