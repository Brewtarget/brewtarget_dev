#
# src/CMakeLists.txt is part of Brewtarget, and is Copyright the following
# authors 2009-2022:
#  - Aidan Roberts <aidanr67@gmail.com>
#  - Daniel Moreno <danielm5@users.noreply.github.com>
#  - Mark de Wever <koraq@xs4all.nl>
#  - Mattias Måhl <mattias@kejsarsten.com>
#  - Matt Young<mfsy@yahoo.com>
#  - Maxime Lavigne <duguigne@gmail.com>
#  - Mik Firestone <mikfire@gmail.com>
#  - Philip Greggory Lee <rocketman768@gmail.com>
#  - Ryan Hoobler <rhoob@yahoo.com>
#  - Théophane Martin <theophane.m@gmail.com>
#
# Brewtarget is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.
#
# Brewtarget is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.
#
# See also ../CMakeLists.txt
#

#
# Uncomment the next line for slightly verbose build output
# Alternatively, for very verbose output, invoke make as follows:
#    $ make VERBOSE=1
#
#set(CMAKE_VERBOSE_MAKEFILE ON)

# The current directory in the original source.
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_SOURCE_DIR} )
# The current directory in the output build directory.
INCLUDE_DIRECTORIES( ${CMAKE_CURRENT_BINARY_DIR} )

# Put all the required Qt dirs in the includes.
#   That isn't quite what this include does. QT_USE_FILE references
#   a cmake macro include, which is good but not required.
#INCLUDE(${QT_USE_FILE})

# (ADD_PCH_RULE  _header_filename _src_list)
# Version 7/26/2010
#
# use this macro before "add_executable"
#
# _header_filename
#  header to make a .gch
#
# _src_list
#   the variable name (do not use ${..}) which contains a
#     a list of sources (a.cpp b.cpp c.cpp ...)
#  This macro will append a header file to it, then this src_list can be used in
#  "add_executable..."
#
#
# Now a .gch file should be generated and gcc should use it.
#        (add -Winvalid-pch to the cpp flags to verify)
#
# make clean should delete the pch file
#
# example : ADD_PCH_RULE(headers.h myprog_SRCS)
MACRO (ADD_PCH_RULE  _header_filename _src_list)
   SET(_gch_filename "${CMAKE_CURRENT_BINARY_DIR}/${_header_filename}.gch")
   SET(_header "${CMAKE_CURRENT_SOURCE_DIR}/${_header_filename}")

   LIST(APPEND ${_src_list} ${_gch_filename})

   SET (_args ${CMAKE_CXX_FLAGS})
   LIST(APPEND _args -c ${_header} -o ${_gch_filename})

   GET_DIRECTORY_PROPERTY(DIRINC INCLUDE_DIRECTORIES)
   FOREACH(_inc ${DIRINC})
      LIST(APPEND _args "-I" ${_inc})
   ENDFOREACH(_inc ${DIRINC})

   SEPARATE_ARGUMENTS(_args)

   ADD_CUSTOM_COMMAND(OUTPUT ${_gch_filename}
                      COMMAND rm -f ${_gch_filename}
                      COMMAND ${CMAKE_CXX_COMPILER} ${CMAKE_CXX_COMPILER_ARG1} ${_args}
                      DEPENDS ${_header})
ENDMACRO(ADD_PCH_RULE _src_list _header_filename)

#
# Variable that contains all the .cpp files in this project.
#
# You can recreate the body of this list by running the following from the bash prompt in the build directory:
#    find ../src -name '*.cpp' | sort  | sed 's+^../src+    ${SRCDIR}+' | grep -v Testing.cpp | grep -v main.cpp
#
SET( brewtarget_SRCS
    ${SRCDIR}/AboutDialog.cpp
    ${SRCDIR}/AlcoholTool.cpp
    ${SRCDIR}/Algorithms.cpp
    ${SRCDIR}/AncestorDialog.cpp
    ${SRCDIR}/BeerColorWidget.cpp
    ${SRCDIR}/boiltime.cpp
    ${SRCDIR}/BrewDayFormatter.cpp
    ${SRCDIR}/BrewDayScrollWidget.cpp
    ${SRCDIR}/BrewDayWidget.cpp
    ${SRCDIR}/BrewNoteWidget.cpp
    ${SRCDIR}/brewtarget.cpp
    ${SRCDIR}/BtColor.cpp
    ${SRCDIR}/BtDatePopup.cpp
    ${SRCDIR}/BtDigitWidget.cpp
    ${SRCDIR}/BtFieldType.cpp
    ${SRCDIR}/BtFolder.cpp
    ${SRCDIR}/BtLabel.cpp
    ${SRCDIR}/BtLineEdit.cpp
    ${SRCDIR}/BtSplashScreen.cpp
    ${SRCDIR}/BtTabWidget.cpp
    ${SRCDIR}/BtTextEdit.cpp
    ${SRCDIR}/BtTreeFilterProxyModel.cpp
    ${SRCDIR}/BtTreeItem.cpp
    ${SRCDIR}/BtTreeModel.cpp
    ${SRCDIR}/BtTreeView.cpp
    ${SRCDIR}/ConverterTool.cpp
    ${SRCDIR}/CustomComboBox.cpp
    ${SRCDIR}/database/BtSqlQuery.cpp
    ${SRCDIR}/database/Database.cpp
    ${SRCDIR}/database/DatabaseSchemaHelper.cpp
    ${SRCDIR}/database/DbTransaction.cpp
    ${SRCDIR}/database/ObjectStore.cpp
    ${SRCDIR}/database/ObjectStoreTyped.cpp
    ${SRCDIR}/EquipmentButton.cpp
    ${SRCDIR}/EquipmentEditor.cpp
    ${SRCDIR}/EquipmentListModel.cpp
    ${SRCDIR}/FermentableDialog.cpp
    ${SRCDIR}/FermentableEditor.cpp
    ${SRCDIR}/FermentableSortFilterProxyModel.cpp
    ${SRCDIR}/HeatCalculations.cpp
    ${SRCDIR}/HopDialog.cpp
    ${SRCDIR}/HopEditor.cpp
    ${SRCDIR}/HopSortFilterProxyModel.cpp
    ${SRCDIR}/Html.cpp
    ${SRCDIR}/HydrometerTool.cpp
    ${SRCDIR}/IbuGuSlider.cpp
    ${SRCDIR}/InstructionWidget.cpp
    ${SRCDIR}/InventoryFormatter.cpp
    ${SRCDIR}/Localization.cpp
    ${SRCDIR}/Logging.cpp
    ${SRCDIR}/MainWindow.cpp
    ${SRCDIR}/MashButton.cpp
    ${SRCDIR}/MashComboBox.cpp
    ${SRCDIR}/MashDesigner.cpp
    ${SRCDIR}/MashEditor.cpp
    ${SRCDIR}/MashListModel.cpp
    ${SRCDIR}/MashStepEditor.cpp
    ${SRCDIR}/MashStepTableWidget.cpp
    ${SRCDIR}/MashWizard.cpp
    ${SRCDIR}/matrix.cpp
    ${SRCDIR}/measurement/Amount.cpp
    ${SRCDIR}/measurement/ColorMethods.cpp
    ${SRCDIR}/measurement/IbuMethods.cpp
    ${SRCDIR}/measurement/Measurement.cpp
    ${SRCDIR}/measurement/PhysicalQuantity.cpp
    ${SRCDIR}/measurement/SystemOfMeasurement.cpp
    ${SRCDIR}/measurement/Unit.cpp
    ${SRCDIR}/measurement/UnitSystem.cpp
    ${SRCDIR}/MiscDialog.cpp
    ${SRCDIR}/MiscEditor.cpp
    ${SRCDIR}/MiscSortFilterProxyModel.cpp
    ${SRCDIR}/model/BrewNote.cpp
    ${SRCDIR}/model/Equipment.cpp
    ${SRCDIR}/model/Fermentable.cpp
    ${SRCDIR}/model/Hop.cpp
    ${SRCDIR}/model/Instruction.cpp
    ${SRCDIR}/model/Inventory.cpp
    ${SRCDIR}/model/Mash.cpp
    ${SRCDIR}/model/MashStep.cpp
    ${SRCDIR}/model/Misc.cpp
    ${SRCDIR}/model/NamedEntity.cpp
    ${SRCDIR}/model/NamedEntityWithInventory.cpp
    ${SRCDIR}/model/NamedParameterBundle.cpp
    ${SRCDIR}/model/Recipe.cpp
    ${SRCDIR}/model/Salt.cpp
    ${SRCDIR}/model/Style.cpp
    ${SRCDIR}/model/Water.cpp
    ${SRCDIR}/model/Yeast.cpp
    ${SRCDIR}/NamedEntitySortProxyModel.cpp
    ${SRCDIR}/NamedMashEditor.cpp
    ${SRCDIR}/OgAdjuster.cpp
    ${SRCDIR}/OptionDialog.cpp
    ${SRCDIR}/PersistentSettings.cpp
    ${SRCDIR}/PitchDialog.cpp
    ${SRCDIR}/PreInstruction.cpp
    ${SRCDIR}/PrimingDialog.cpp
    ${SRCDIR}/PrintAndPreviewDialog.cpp
    ${SRCDIR}/RadarChart.cpp
    ${SRCDIR}/RangedSlider.cpp
    ${SRCDIR}/RecipeExtrasWidget.cpp
    ${SRCDIR}/RecipeFormatter.cpp
    ${SRCDIR}/RefractoDialog.cpp
    ${SRCDIR}/ScaleRecipeTool.cpp
    ${SRCDIR}/SimpleUndoableUpdate.cpp
    ${SRCDIR}/StrikeWaterDialog.cpp
    ${SRCDIR}/StyleButton.cpp
    ${SRCDIR}/StyleEditor.cpp
    ${SRCDIR}/StyleListModel.cpp
    ${SRCDIR}/StyleRangeWidget.cpp
    ${SRCDIR}/StyleSortFilterProxyModel.cpp
    ${SRCDIR}/tableModels/BtTableModel.cpp
    ${SRCDIR}/tableModels/BtTableModelInventory.cpp
    ${SRCDIR}/tableModels/FermentableTableModel.cpp
    ${SRCDIR}/tableModels/HopTableModel.cpp
    ${SRCDIR}/tableModels/MashStepTableModel.cpp
    ${SRCDIR}/tableModels/MiscTableModel.cpp
    ${SRCDIR}/tableModels/SaltTableModel.cpp
    ${SRCDIR}/tableModels/WaterTableModel.cpp
    ${SRCDIR}/tableModels/YeastTableModel.cpp
    ${SRCDIR}/TimerListDialog.cpp
    ${SRCDIR}/TimerMainDialog.cpp
    ${SRCDIR}/TimerWidget.cpp
    ${SRCDIR}/UiAmountWithUnits.cpp
    ${SRCDIR}/utils/BtException.cpp
    ${SRCDIR}/utils/BtStringConst.cpp
    ${SRCDIR}/utils/BtStringStream.cpp
    ${SRCDIR}/utils/EnumStringMapping.cpp
    ${SRCDIR}/utils/TimerUtils.cpp
    ${SRCDIR}/WaterButton.cpp
    ${SRCDIR}/WaterDialog.cpp
    ${SRCDIR}/WaterEditor.cpp
    ${SRCDIR}/WaterListModel.cpp
    ${SRCDIR}/WaterSortFilterProxyModel.cpp
    ${SRCDIR}/WaterTableWidget.cpp
    ${SRCDIR}/widgets/Animator.cpp
    ${SRCDIR}/widgets/SelectionControl.cpp
    ${SRCDIR}/widgets/ToggleSwitch.cpp
    ${SRCDIR}/widgets/UnitAndScalePopUpMenu.cpp
    ${SRCDIR}/xml/BeerXml.cpp
    ${SRCDIR}/xml/BtDomErrorHandler.cpp
    ${SRCDIR}/xml/XercesHelpers.cpp
    ${SRCDIR}/xml/XmlCoding.cpp
    ${SRCDIR}/xml/XmlMashRecord.cpp
    ${SRCDIR}/xml/XmlMashStepRecord.cpp
    ${SRCDIR}/xml/XmlRecipeRecord.cpp
    ${SRCDIR}/xml/XmlRecordCount.cpp
    ${SRCDIR}/xml/XmlRecord.cpp
    ${SRCDIR}/YeastDialog.cpp
    ${SRCDIR}/YeastEditor.cpp
    ${SRCDIR}/YeastSortFilterProxyModel.cpp
)

# List of all the *.ui
# TODO: can I somehow have a separate CMakeLists.txt
# in the ui/ directory instead of here?
#
# You can recreate the body of this list by running the following from the bash prompt in the build directory:
#    find ../ui -name '*.ui' | sort  | sed 's+^../ui+    ${SRCDIR}+'
#
SET( brewtarget_UIS
    ${UIDIR}/ancestorDialog.ui
    ${UIDIR}/brewDayScrollWidget.ui
    ${UIDIR}/brewDayWidget.ui
    ${UIDIR}/brewNoteWidget.ui
    ${UIDIR}/BtPrintAndPreview.ui
    ${UIDIR}/equipmentEditor.ui
    ${UIDIR}/fermentableEditor.ui
    ${UIDIR}/hopEditor.ui
    ${UIDIR}/instructionWidget.ui
    ${UIDIR}/mainWindow.ui
    ${UIDIR}/mashDesigner.ui
    ${UIDIR}/mashEditor.ui
    ${UIDIR}/mashStepEditor.ui
    ${UIDIR}/mashWizard.ui
    ${UIDIR}/miscEditor.ui
    ${UIDIR}/namedMashEditor.ui
    ${UIDIR}/ogAdjuster.ui
    ${UIDIR}/optionsDialog.ui
    ${UIDIR}/pitchDialog.ui
    ${UIDIR}/primingDialog.ui
    ${UIDIR}/recipeExtrasWidget.ui
    ${UIDIR}/refractoDialog.ui
    ${UIDIR}/strikeWaterDialog.ui
    ${UIDIR}/styleEditor.ui
    ${UIDIR}/timerMainDialog.ui
    ${UIDIR}/timerWidget.ui
    ${UIDIR}/waterDialog.ui
    ${UIDIR}/waterEditor.ui
    ${UIDIR}/yeastEditor.ui
)

#
# The list of files that need to be moc'd.
# In other words, classes that have Q_OBJECT declarations and stuff.
# NB: This is NOT a list of ALL header files!
#
# You can recreate the body of this list by running the following from the bash prompt in the build directory:
#    grep -rl Q_OBJECT ../src | sort | sed 's+^../src+    ${SRCDIR}+' | grep -v Testing.h
#
SET( brewtarget_MOC_HEADERS
    ${SRCDIR}/AboutDialog.h
    ${SRCDIR}/AlcoholTool.h
    ${SRCDIR}/AncestorDialog.h
    ${SRCDIR}/BeerColorWidget.h
    ${SRCDIR}/boiltime.h
    ${SRCDIR}/BrewDayFormatter.h
    ${SRCDIR}/BrewDayScrollWidget.h
    ${SRCDIR}/BrewDayWidget.h
    ${SRCDIR}/brewtarget.h
    ${SRCDIR}/BrewNoteWidget.h
    ${SRCDIR}/BtDatePopup.h
    ${SRCDIR}/BtDigitWidget.h
    ${SRCDIR}/BtFolder.h
    ${SRCDIR}/BtLabel.h
    ${SRCDIR}/BtLineEdit.h
    ${SRCDIR}/BtSplashScreen.h
    ${SRCDIR}/BtTabWidget.h
    ${SRCDIR}/BtTextEdit.h
    ${SRCDIR}/BtTreeFilterProxyModel.h
    ${SRCDIR}/BtTreeModel.h
    ${SRCDIR}/BtTreeView.h
    ${SRCDIR}/CMakeLists.txt
    ${SRCDIR}/ConverterTool.h
    ${SRCDIR}/CustomComboBox.h
    ${SRCDIR}/database/ObjectStore.h
    ${SRCDIR}/EquipmentButton.h
    ${SRCDIR}/EquipmentEditor.h
    ${SRCDIR}/EquipmentListModel.h
    ${SRCDIR}/FermentableDialog.h
    ${SRCDIR}/FermentableEditor.h
    ${SRCDIR}/FermentableSortFilterProxyModel.h
    ${SRCDIR}/HopDialog.h
    ${SRCDIR}/HopEditor.h
    ${SRCDIR}/HopSortFilterProxyModel.h
    ${SRCDIR}/HydrometerTool.h
    ${SRCDIR}/IbuGuSlider.h
    ${SRCDIR}/InstructionWidget.h
    ${SRCDIR}/MainWindow.h
    ${SRCDIR}/MashButton.h
    ${SRCDIR}/MashComboBox.h
    ${SRCDIR}/MashDesigner.h
    ${SRCDIR}/MashEditor.h
    ${SRCDIR}/MashListModel.h
    ${SRCDIR}/MashStepEditor.h
    ${SRCDIR}/MashStepTableWidget.h
    ${SRCDIR}/MashWizard.h
    ${SRCDIR}/measurement/Unit.h
    ${SRCDIR}/MiscDialog.h
    ${SRCDIR}/MiscEditor.h
    ${SRCDIR}/MiscSortFilterProxyModel.h
    ${SRCDIR}/model/BrewNote.h
    ${SRCDIR}/model/Equipment.h
    ${SRCDIR}/model/Fermentable.h
    ${SRCDIR}/model/Hop.h
    ${SRCDIR}/model/Instruction.h
    ${SRCDIR}/model/Inventory.h
    ${SRCDIR}/model/Mash.h
    ${SRCDIR}/model/MashStep.h
    ${SRCDIR}/model/Misc.h
    ${SRCDIR}/model/NamedEntity.h
    ${SRCDIR}/model/NamedEntityWithInventory.h
    ${SRCDIR}/model/Recipe.h
    ${SRCDIR}/model/Salt.h
    ${SRCDIR}/model/Style.h
    ${SRCDIR}/model/Water.h
    ${SRCDIR}/model/Yeast.h
    ${SRCDIR}/NamedEntitySortProxyModel.h
    ${SRCDIR}/NamedMashEditor.h
    ${SRCDIR}/OgAdjuster.h
    ${SRCDIR}/OptionDialog.h
    ${SRCDIR}/PitchDialog.h
    ${SRCDIR}/PrimingDialog.h
    ${SRCDIR}/PrintAndPreviewDialog.h
    ${SRCDIR}/RangedSlider.h
    ${SRCDIR}/RecipeExtrasWidget.h
    ${SRCDIR}/RecipeFormatter.h
    ${SRCDIR}/RefractoDialog.h
    ${SRCDIR}/ScaleRecipeTool.h
    ${SRCDIR}/SimpleUndoableUpdate.h
    ${SRCDIR}/StrikeWaterDialog.h
    ${SRCDIR}/StyleButton.h
    ${SRCDIR}/StyleEditor.h
    ${SRCDIR}/StyleListModel.h
    ${SRCDIR}/StyleRangeWidget.h
    ${SRCDIR}/StyleSortFilterProxyModel.h
    ${SRCDIR}/tableModels/BtTableModel.h
    ${SRCDIR}/tableModels/FermentableTableModel.h
    ${SRCDIR}/tableModels/HopTableModel.h
    ${SRCDIR}/tableModels/MashStepTableModel.h
    ${SRCDIR}/tableModels/MiscTableModel.h
    ${SRCDIR}/tableModels/SaltTableModel.h
    ${SRCDIR}/tableModels/WaterTableModel.h
    ${SRCDIR}/tableModels/YeastTableModel.h
    ${SRCDIR}/TimerListDialog.h
    ${SRCDIR}/TimerMainDialog.h
    ${SRCDIR}/TimerWidget.h
    ${SRCDIR}/WaterButton.h
    ${SRCDIR}/WaterDialog.h
    ${SRCDIR}/WaterEditor.h
    ${SRCDIR}/WaterListModel.h
    ${SRCDIR}/WaterSortFilterProxyModel.h
    ${SRCDIR}/WaterTableWidget.h
    ${SRCDIR}/widgets/Animator.h
    ${SRCDIR}/widgets/SelectionControl.h
    ${SRCDIR}/widgets/ToggleSwitch.h
    ${SRCDIR}/widgets/UnitAndScalePopUpMenu.h
    ${SRCDIR}/YeastDialog.h
    ${SRCDIR}/YeastEditor.h
    ${SRCDIR}/YeastSortFilterProxyModel.h
)

SET( FILES_TO_TRANSLATE
     ${brewtarget_SRCS}
     ${brewtarget_UIS}
)

#======Speed up compilation by using precompiled headers for development======
#IF( UNIX AND NOT APPLE )
#   SET( precompiled_h "equipment.h" "fermentable.h" "hop.h" "instruction.h" "mash.h" "mashstep.h" "misc.h" "recipe.h" "style.h" "unit.h" "water.h" "yeast.h" "database.h" "brewtarget.h" )
#   FOREACH( header ${precompiled_h} )
#      ADD_PCH_RULE( ${header} brewtarget_SRCS )
#   ENDFOREACH()
#ENDIF()

#==================================Qt Junk=====================================

# Create the ui_*.h files from the *.ui files.
QT5_WRAP_UI(brewtarget_UIS_H ${brewtarget_UIS})
# Generate the resource .cpp from the .qrc file.
QT5_ADD_RESOURCES(brewtarget_QRC_CPP ${brewtarget_QRC})
QT5_ADD_RESOURCES(brewtarget_cfg_QRC_CPP ${brewtarget_cfg_QRC})

# Translations. CREATE_TRANSLATION will update the .ts files (do not want)
#QT5_CREATE_TRANSLATION(${TRANSLATIONS_FILES} ${FILES_TO_TRANSLATE})
# Creates the .qm files from the .ts files
QT5_ADD_TRANSLATION(QM_FILES ${TRANSLATIONS_FILES})

# Add a target for the QM_FILES so that we can add
# the translations as a dependency for the executable later.
ADD_CUSTOM_TARGET(translations DEPENDS ${QM_FILES})

#============================Icon for Windows==================================

SET(brewtarget_ICON "")

IF( WIN32 AND MINGW )
  ADD_CUSTOM_COMMAND(OUTPUT ${CMAKE_BINARY_DIR}/src/icon.o
                     COMMAND windres.exe -I${CMAKE_CURRENT_SOURCE_DIR}
                     -i${RCFILE}
                     -o${CMAKE_BINARY_DIR}/src/icon.o
                     DEPENDS ${RCFILE}
  )
  SET(brewtarget_ICON ${CMAKE_BINARY_DIR}/src/icon.o)
ELSEIF(WIN32)
  SET(brewtarget_ICON ${RCFILE})
ENDIF()

#===========================================Requirements for Boost Stacktrace===========================================
#
# Per https://www.boost.org/doc/libs/1_76_0/doc/html/stacktrace/configuration_and_build.html, by default
# Boost.Stacktrace is a header-only library.  However, you get better results by linking (either statically or
# dynamically) with a helper library.  Of the various options, it seems like boost_stacktrace_backtrace gives the most
# functionality over the most platforms.  This has dependencies on:
#   - libdl on POSIX platforms -- but see note below
#   - libbacktrace
# The latter is an external library on Windows.  On POSIX plaforms it's typically already either installed on the system
# (eg see https://man7.org/linux/man-pages/man3/backtrace.3.html) or built in to the compiler.  Fortunately, CMake knows
# how to do the right thing in either case, thanks to https://cmake.org/cmake/help/latest/module/FindBacktrace.html.
#
# Just to make things extra fun, in 2021, the GNU compilers did away with libdl and incorporated its functionality into
# libc, per the announcement of GNU C Library v2.3.4 at
# https://sourceware.org/pipermail/libc-alpha/2021-August/129718.html.  This means, if we're using the GNU tool chain
# and libc is v2.3.4 or newer, then we should NOT look for libdl, as we won't find it!
#
# Fortunately, CMake has a special variable, CMAKE_DL_LIBS, that is, essentially "whatever library you need to link to
# for dlopen and dlclose", so we don't need to worry about libc versions.
#
IF(NOT WIN32)
   set(DL_LIBRARY ${CMAKE_DL_LIBS})
ENDIF()
find_package(Backtrace REQUIRED)
# For the moment, leave default settings for Mac as can't work out how to get the backtrace version of stacktrace
# working.  (Should still get stack traces on Mac, but might not get as much info in them as we'd like.)
IF(NOT APPLE)
   IF(NOT WIN32)
# TBD Some users report problems getting CMake to find libboost_stacktrace_backtrace on Ubuntu and Gentoo, so disable it
# for now and fallback to the header-only version
#      add_compile_definitions(BOOST_STACKTRACE_DYN_LINK)
   ENDIF()
#   add_compile_definitions(BOOST_STACKTRACE_USE_BACKTRACE)
ENDIF()
message("Backtrace libs " ${DL_LIBRARY} " | " ${Backtrace_LIBRARIES} " | " ${Boost_LIBRARIES})

#===========================Create the binary==================================

# This creates a "library" of object files so that we do not have to recompile
# the source files once per target, but rather, just once EVER.
ADD_LIBRARY(
   btobjlib
   OBJECT
   ${brewtarget_SRCS}
   ${brewtarget_MOC_SRCS}
   ${brewtarget_QRC_CPP}
   ${brewtarget_UIS_H}
)

IF( APPLE )
   # It seems I have to set these properties in this file, or things don't
   # get installed
   SET_SOURCE_FILES_PROPERTIES(
      ${brewtarget_MAC_ICNS}
      PROPERTIES
      MACOSX_PACKAGE_LOCATION "Resources"
   )
   SET_SOURCE_FILES_PROPERTIES(
      ${brewtarget_DATA}
      PROPERTIES
      MACOSX_PACKAGE_LOCATION "Resources"
   )
   SET_SOURCE_FILES_PROPERTIES(
      ${brewtarget_DOCS}
      PROPERTIES
      MACOSX_PACKAGE_LOCATION "Resources/en.lproj"
   )
   SET_SOURCE_FILES_PROPERTIES(
      ${brewtarget_SOUNDS}
      PROPERTIES
      MACOSX_PACKAGE_LOCATION "Resources/sounds"
   )
   SET_SOURCE_FILES_PROPERTIES(
      ${QM_FILES}
      PROPERTIES
      MACOSX_PACKAGE_LOCATION "Resources/translations_qm"
   )
   ADD_EXECUTABLE(
      ${brewtarget_EXECUTABLE}
      MACOSX_BUNDLE
      ${SRCDIR}/main.cpp
      ${brewtarget_MAC_ICNS}
      ${brewtarget_DATA}
      ${brewtarget_DOCS}
      ${brewtarget_SOUNDS}
      ${QM_FILES}
      $<TARGET_OBJECTS:btobjlib>
   )
ELSE()
   ADD_EXECUTABLE(
      ${brewtarget_EXECUTABLE}
      ${SRCDIR}/main.cpp
      ${brewtarget_ICON}
      $<TARGET_OBJECTS:btobjlib>
   )
ENDIF()

ADD_DEPENDENCIES( ${brewtarget_EXECUTABLE} translations )

# Link brewtarget against appropriate libraries.
IF( WIN32 AND MINGW )
   ############################################################################
   # Need to set some linker flags that I don't know how to get
   # automatically.
   ############################################################################

   # MinGW-specific flags.
   #    '-Wl,-subsystem,windows' supresses the output command window.
   #    '-Wl,-s' strips the executable of symbols.
   SET_TARGET_PROPERTIES( ${brewtarget_EXECUTABLE} PROPERTIES LINK_FLAGS "-Wl,-enable-stdcall-fixup -Wl,-enable-auto-import -Wl,-enable-runtime-pseudo-reloc -mthreads -Wl,-subsystem,windows")

   if(TARGET Qt5::windeployqt)
      # execute windeployqt in a tmp directory after build
      add_custom_command(TARGET ${brewtarget_EXECUTABLE}
         POST_BUILD
         COMMAND ${CMAKE_COMMAND} -E remove_directory "${CMAKE_CURRENT_BINARY_DIR}/windeployqt"
         COMMAND Qt5::windeployqt --dir "${CMAKE_CURRENT_BINARY_DIR}/windeployqt" --no-translations --compiler-runtime "$<TARGET_FILE_DIR:${brewtarget_EXECUTABLE}>/$<TARGET_FILE_NAME:${brewtarget_EXECUTABLE}>"
   )

   # copy deployment directory during installation
   install(
      DIRECTORY
      "${CMAKE_CURRENT_BINARY_DIR}/windeployqt/"
      DESTINATION bin
   )
   endif()
ENDIF()

SET( QT5_USE_MODULES_LIST
   ${brewtarget_EXECUTABLE}
   Qt5::Widgets
   Qt5::Network
   Qt5::PrintSupport
   Qt5::Sql
   Qt5::Svg
   Qt5::Xml
   )

SET( QT5_USE_MODULES_LIST ${QT5_USE_MODULES_LIST} Qt5::Multimedia)

#
# All the libraries
#
# ********************************************************************************************************************
# * Note that if you change this you need to make corresponding changes to this to target_link_libraries in the Tests
# * section below.
# *
# * .:TODO:. Work out how to avoid this copy-and-paste duplication
# ********************************************************************************************************************
#
target_link_libraries(
   ${QT5_USE_MODULES_LIST}
   ${XercesC_LIBRARIES}
   ${XalanC_LIBRARIES}
   ${Boost_LIBRARIES}
   ${DL_LIBRARY}
   ${Backtrace_LIBRARIES}
)

#=================================Tests========================================

#QT4_WRAP_CPP( testing_MOC_SRCS ${SRCDIR}/unitTests/Testing.h )
ADD_EXECUTABLE(
   brewtarget_tests
   ${SRCDIR}/unitTests/Testing.cpp
   ${testing_MOC_SRCS}
   $<TARGET_OBJECTS:btobjlib>
)

SET( QT5_USE_MODULES_LIST
   brewtarget_tests
   Qt5::Widgets
   Qt5::Network
   Qt5::PrintSupport
   Qt5::Sql
   Qt5::Svg
   Qt5::Xml
   Qt5::Test
   )

SET( QT5_USE_MODULES_LIST ${QT5_USE_MODULES_LIST} Qt5::Multimedia)

# NB Needs to be same as the one a few lines above. Sigh.
target_link_libraries(
   ${QT5_USE_MODULES_LIST}
   ${XercesC_LIBRARIES}
   ${XalanC_LIBRARIES}
   ${Boost_LIBRARIES}
   ${DL_LIBRARY}
   ${Backtrace_LIBRARIES}
)

ADD_TEST(
   NAME pstdintTest
   COMMAND brewtarget_tests pstdintTest
)
ADD_TEST(
   NAME recipeCalcTest_allGrain
   COMMAND brewtarget_tests recipeCalcTest_allGrain
)
ADD_TEST(
   NAME postBoilLossOgTest
   COMMAND brewtarget_tests postBoilLossOgTest
)
ADD_TEST(
   NAME testUnitConversions
   COMMAND brewtarget_tests testUnitConversions
)
add_test(
   NAME testLogRotation
   COMMAND brewtarget_tests testLogRotation
)
#=================================Installs=====================================

# Install executable.
INSTALL( TARGETS ${brewtarget_EXECUTABLE}
         BUNDLE DESTINATION .
         RUNTIME DESTINATION ${TARGETPATH}
         COMPONENT ${RUNTIME_INSTALL_COMPONENT} )

IF( WIN32 )
      install(CODE
      "
         file(GET_RUNTIME_DEPENDENCIES
            RESOLVED_DEPENDENCIES_VAR deps
            UNRESOLVED_DEPENDENCIES_VAR confdeps
            EXECUTABLES src/${brewtarget_EXECUTABLE}.exe
            PRE_EXCLUDE_REGEXES \"^api-ms-win.*\$|^ext-ms.*\$\"
            POST_EXCLUDE_REGEXES \".*system32.*\"
            DIRECTORIES ${Mingw_Path}
         )
         #message(\"Installing: \${deps}\")
         #message(\"Not found: \${confdeps}\")
         FOREACH (fdep IN LISTS deps)
            message( \"\${fdep}\" )
            file( INSTALL
               DESTINATION bin
               TYPE SHARED_LIBRARY
               FOLLOW_SYMLINK_CHAIN
               FILES \"\${fdep}\")
         ENDFOREACH()
      ")
   ENDIF()

# Install the translations.
INSTALL(FILES ${QM_FILES}
        DESTINATION "${DATAPATH}/translations_qm"
        COMPONENT ${DATA_INSTALL_COMPONENT} )

# http://pmarinc-tidylib.googlecode.com/hg/src/Sigil/CMakeLists.txt?r=8276c61e05bc385d5ffbcc58e1f007f84b0c52df
IF( APPLE )
   SET ( BOOST_GNU_SOURCE_FLAG "-D_GNU_SOURCE" )
   SET ( CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${BOOST_GNU_SOURCE_FLAG}" )
   # make addframeworks should copy the Qt libraries to the app.
   SET(QT_BINARY_DIR "${_qt5Core_install_prefix}/bin")
   ADD_CUSTOM_TARGET(
      addframeworks ALL
      COMMAND ${QT_BINARY_DIR}/macdeployqt "Brewtarget.app" -dmg
      WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
      DEPENDS ${brewtarget_EXECUTABLE}
   )
ENDIF()
