PROJECT(shapeAnalysisMANCOVA)

CMAKE_MINIMUM_REQUIRED(VERSION 2.4)

# Find ITK.
FIND_PACKAGE(ITK REQUIRED)
IF (ITK_FOUND)
   INCLUDE(${USE_ITK_FILE})
ELSE(ITK_FOUND)
   MESSAGE(FATAL_ERROR, "ITK not found. Please set ITK_DIR.")
ENDIF (ITK_FOUND)

# Find VTK.
FIND_PACKAGE(VTK REQUIRED)
IF (VTK_FOUND)
   INCLUDE(${USE_VTK_FILE})
ELSE(VTK_FOUND)
   MESSAGE(FATAL_ERROR, "VTK not found. Please set VTK_DIR.")
ENDIF (VTK_FOUND)

FIND_PATH(BOOST_INCLUDE_DIR boost/config.hpp)
INCLUDE_DIRECTORIES(${BOOST_INCLUDE_DIR})

FIND_PATH(BOOST_LIBRARY_DIR boost_date_time-vc71-mt.lib)
LINK_DIRECTORIES(${BOOST_LIBRARY_DIR})

# Win32 has automatic linking of boost libraries so only add boost
# libaries on unix
IF(NOT WIN32 OR CYGWIN)
  SET(BOOST_LIBRARIES boost_program_options boost_filesystem boost_system-mt)
ENDIF(NOT WIN32 OR CYGWIN)

FIND_PACKAGE(GenerateCLP REQUIRED)
IF(GenerateCLP_FOUND)
  INCLUDE(${GenerateCLP_USE_FILE})

 #SET (shapeAnalysisMANCOVA_SRCS shapeAnalysisMANCOVA.cxx MANCOVA.cxx itkMeshTovtkPolyData.cxx miscMANCOVA.cxx nonMANCOVA.cxx)

  SET(shapeAnalysisMANCOVA_SRCS 
    shapeAnalysisMANCOVA.cxx 
    MANCOVA.cxx  
    itkMeshTovtkPolyData.h
    itkMeshTovtkPolyData.cxx 
    miscMANCOVA.cxx 
    nonMANCOVA.cxx 
  )
  
  GENERATECLP(${shapeAnalysisMANCOVA_SRCS} shapeAnalysisMANCOVA.xml)
  ADD_EXECUTABLE(shapeAnalysisMANCOVA ${shapeAnalysisMANCOVA_SRCS})

  TARGET_LINK_LIBRARIES(shapeAnalysisMANCOVA
    ${ITK_LIBRARIES} 
    ITKCommon 
    ITKAlgorithms  
    vtkGraphics
    vtkIO
    ITKIO
    vtkCommon
    vtkFiltering
    ${BOOST_LIBRARIES}	
    )	
ENDIF(GenerateCLP_FOUND)


