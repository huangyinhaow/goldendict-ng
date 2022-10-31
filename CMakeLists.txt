cmake_minimum_required(VERSION 3.16)
project(goldendict 
        VERSION 22.9.24.0
        LANGUAGES CXX
        DESCRIPTION "A feature-rich dictionary lookup program, supporting multiple dictionary formats and online dictionaries"
)

# todo program version
# set(PROGRAM_VERSION "${PROJECT_VERSION}${PROGRAM_VERSION_SUFFIX}")
# add_definitions(-DPROGRAM_VERSION=\"${PROGRAM_VERSION}\")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
if(MSVC)
    set(CMAKE_CXX_EXTENSIONS OFF)
endif()
set(CMAKE_WIN32_EXECUTABLE ON)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOMOC ON)
set(CMAKE_AUTORCC ON)
set(CMAKE_AUTOUIC ON)

option(WITH_EXTRA_TIFF_HANDLER "Enable extra TIFF handler" OFF)
option(WITH_QTMULTIMEDIA_PLAYER "Enable support for QtMultiMedia player" ON)
option(WITH_FFMPEG_PLAYER "Enable support for FFMPEG player" ON)
option(WITH_EPWING "Enable support for Epwing dictionary format" OFF)
option(WITH_ZIM "Enable support for ZIM Wiki dictionary" ON)
option(WITH_OPENCC "Enable support of Open Chinese Conversion" ON)
option(WITH_use_xapian "use xapian" OFF)

option(WITH_no_macos_universal "no macos universal" OFF)

find_package(QT NAMES Qt5 Qt6 REQUIRED COMPONENTS Core)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS Concurrent Gui Help Network PrintSupport Svg WebChannel WebEngineWidgets Widgets Xml)
find_package(Qt${QT_VERSION_MAJOR} OPTIONAL_COMPONENTS Core5Compat X11Extras)
find_package(Hunspell)
find_package(WrapZSTD 1.3)

include(GNUInstallDirs)


find_package(PkgConfig REQUIRED)


pkg_check_modules(linux_base_libs REQUIRED IMPORTED_TARGET
    "vorbisfile" "vorbis" "ogg" "hunspell"
)

if(WITH_FFMPEG_PLAYER)
    pkg_check_modules(ffmpeg_libs REQUIRED IMPORTED_TARGET
        "ao" "libavutil" "libavformat" "libavcodec" "libswresample"
    )
endif()


if(WITH_QTMULTIMEDIA_PLAYER)
find_package(Qt${QT_VERSION_MAJOR} REQUIRED COMPONENTS
    Multimedia
)
endif()
if(WITH_EXTRA_TIFF_HANDLER)
    find_package(TIFF)
endif()

if(WITH_ZIM)
    find_package(LibLZMA)
endif()



qt_standard_project_setup()

qt_add_executable(goldendict WIN32 MACOSX_BUNDLE
    aard.cc aard.hh
    about.cc about.hh about.ui
    ankiconnector.cpp ankiconnector.h
    article_inspect.cpp article_inspect.h
    article_maker.cc article_maker.hh
    article_netmgr.cc article_netmgr.hh
    articleview.cc articleview.hh articleview.ui
    articlewebpage.cpp articlewebpage.h
    articlewebview.cc articlewebview.hh
    atomic_rename.cc atomic_rename.hh
    audiolink.cc audiolink.hh
    audioplayerfactory.cc audioplayerfactory.hh
    audioplayerinterface.hh
    authentication.ui
    base/globalregex.cc base/globalregex.hh
    belarusiantranslit.cc belarusiantranslit.hh
    bgl.cc bgl.hh
    bgl_babylon.cc bgl_babylon.hh
    btreeidx.cc btreeidx.hh
    chunkedstorage.cc chunkedstorage.hh
    config.cc config.hh
    country.cc country.hh
    decompress.cc decompress.hh
    delegate.cc delegate.hh
    dictdfiles.cc dictdfiles.hh
    dictgroupwidget.ui
    dictheadwords.cc dictheadwords.hh dictheadwords.ui
    dictinfo.cc dictinfo.hh dictinfo.ui
    dictionary.cc dictionary.hh
    dictionarybar.cc dictionarybar.hh
    dictserver.cc dictserver.hh
    dictspanewidget.hh
    dictzip.c dictzip.h
    dsl.cc dsl.hh
    dsl_details.cc dsl_details.hh
    editdictionaries.cc editdictionaries.hh editdictionaries.ui
    ex.hh
    externalaudioplayer.cc externalaudioplayer.hh
    externalviewer.cc externalviewer.hh
    extlineedit.cc extlineedit.hh
    favoritespanewidget.cc favoritespanewidget.hh
    ffmpegaudio.cc ffmpegaudio.hh
    ffmpegaudioplayer.hh
    file.cc file.hh
    filetype.cc filetype.hh
    folding.cc folding.hh
    forvo.cc forvo.hh
    fsencoding.cc fsencoding.hh
    ftshelpers.cc ftshelpers.hh
    fulltextsearch.cc fulltextsearch.hh fulltextsearch.ui
    gdappstyle.cc gdappstyle.hh
    gddebug.cc gddebug.hh
    german.cc german.hh
    gestures.cc gestures.hh
    globalbroadcaster.cpp globalbroadcaster.h
    gls.cc gls.hh
    greektranslit.cc greektranslit.hh
    groupcombobox.cc groupcombobox.hh
    groups.cc groups.hh groups.ui
    groups_widgets.cc groups_widgets.hh
    headwordsmodel.cpp headwordsmodel.h
    helpwindow.cc helpwindow.hh
    history.cc history.hh
    historypanewidget.cc historypanewidget.hh
    hotkeyedit.cc hotkeyedit.hh
    hotkeywrapper.cc hotkeywrapper.hh
    htmlescape.cc htmlescape.hh
    hunspell.cc hunspell.hh
    iconv.cc iconv.hh
    iframeschemehandler.cpp iframeschemehandler.h
    inc_case_folding.hh
    inc_diacritic_folding.hh
    indexedzip.cc indexedzip.hh
    initializing.cc initializing.hh initializing.ui
    instances.cc instances.hh
    keyboardstate.cc keyboardstate.hh
    langcoder.cc langcoder.hh
    language.cc language.hh
    loaddictionaries.cc loaddictionaries.hh
    lsa.cc lsa.hh
    main.cc
    mainstatusbar.cc mainstatusbar.hh
    maintabwidget.cc maintabwidget.hh
    mainwindow.cc mainwindow.hh mainwindow.ui
    mdictparser.cc mdictparser.hh
    mdx.cc mdx.hh
    mediawiki.cc mediawiki.hh
    mruqmenu.cc mruqmenu.hh
    multimediaaudioplayer.cc multimediaaudioplayer.hh
    mutex.cc mutex.hh
    orderandprops.cc orderandprops.hh orderandprops.ui
    parsecmdline.cc parsecmdline.hh
    preferences.cc preferences.hh preferences.ui
    processwrapper.cc processwrapper.hh
    programs.cc programs.hh
    resourceschemehandler.cpp resourceschemehandler.h
    ripemd.cc ripemd.hh
    romaji.cc romaji.hh
    russiantranslit.cc russiantranslit.hh
    scanpopup.cc scanpopup.hh scanpopup.ui
    sdict.cc sdict.hh
    searchpanewidget.hh
    slob.cc slob.hh
    sounddir.cc sounddir.hh
    sources.cc sources.hh sources.ui
    splitfile.cc splitfile.hh
    sptr.hh
    stardict.cc stardict.hh
    stylescombobox.cc stylescombobox.hh
    termination.cc termination.hh
    thirdparty/qtsingleapplication/src/qtlocalpeer.cpp thirdparty/qtsingleapplication/src/qtlocalpeer.h
    thirdparty/qtsingleapplication/src/qtsingleapplication.cpp thirdparty/qtsingleapplication/src/qtsingleapplication.h
    tiff.cc tiff.hh
    translatebox.cc translatebox.hh
    transliteration.cc transliteration.hh
    treeview.cc treeview.hh
    ufile.cc ufile.hh
    utf8.cc utf8.hh
    utils.cc utils.hh
    voiceengines.cc voiceengines.hh
    webmultimediadownload.cc webmultimediadownload.hh
    website.cc website.hh
    weburlrequestinterceptor.cpp weburlrequestinterceptor.h
    wildcard.cc wildcard.hh
    wordfinder.cc wordfinder.hh
    wordlist.cc wordlist.hh
    wstring.hh
    wstring_qt.cc wstring_qt.hh
    xdxf.cc xdxf.hh
    xdxf2html.cc xdxf2html.hh
    zim.cc zim.hh
    zipfile.cc zipfile.hh
    zipsounds.cc zipsounds.hh
)
target_include_directories(goldendict PRIVATE
    .
    thirdparty/qtsingleapplication/src
)

if(WITH_EPWING)
    target_sources(${PROJECT_NAME} PUBLIC
        epwing.cc
        epwing_book.cc
        epwing_charmap.cc
    )
endif()

if(WITH_OPENCC)
    target_sources(${PROJECT_NAME} PUBLIC
        chinese.cc
        chineseconversion.cc
        chineseconversion.ui
    )
endif()
if(UNIX)
    target_sources(${PROJECT_NAME} PUBLIC
        scanflag.cc
        scanflag.ui
    )
endif()

target_compile_definitions(goldendict PRIVATE
    PROGRAM_VERSION=\\\"22.9.24-alpha\\\"
    QT_DISABLE_DEPRECATED_BEFORE=0x050F00
)

target_link_libraries(goldendict PRIVATE
    Qt::Concurrent
    Qt::Core
    Qt::Gui
    Qt::Help
    Qt::Network
    Qt::PrintSupport
    Qt::Svg
    Qt::WebChannel
    Qt::WebEngineWidgets
    Qt::Widgets
    Qt::Xml
    bz2
    lzo2
    z
)


# Resources:
set(resources_resource_files
    "CREDITS.txt"
    "article-style-print.css"
    "article-style-st-babylon.css"
    "article-style-st-classic.css"
    "article-style-st-lingoes-blue.css"
    "article-style-st-lingoes.css"
    "article-style-st-lingvo.css"
    "article-style-st-modern.css"
    "article-style.css"
    "icons/1downarrow.svg"
    "icons/addtab.svg"
    "icons/advanced.svg"
    "icons/arrow.png"
    "icons/blank.png"
    "icons/book.svg"
    "icons/bookcase.svg"
    "icons/clear.png"
    "icons/closetab-hover.png"
    "icons/closetab.svg"
    "icons/collapse_article.png"
    "icons/collapse_article_hovered.png"
    "icons/collapse_opt.png"
    "icons/configure.svg"
    "icons/dictionary.svg"
    "icons/downarrow.png"
    "icons/error.svg"
    "icons/expand_article.png"
    "icons/expand_article_hovered.png"
    "icons/expand_opt.png"
    "icons/fileopen.svg"
    "icons/filesave.png"
    "icons/folder-sound.svg"
    "icons/folder.png"
    "icons/folder.svg"
    "icons/folders.svg"
    "icons/forvo.png"
    "icons/fulltext.svg"
    "icons/highlighter.png"
    "icons/home.svg"
    "icons/hotkeys.svg"
    "icons/icon.svg"
    "icons/icon32_aard.png"
    "icons/icon32_bgl.png"
    "icons/icon32_dictd.png"
    "icons/icon32_dsl.png"
    "icons/icon32_epwing.png"
    "icons/icon32_gls.png"
    "icons/icon32_hunspell.png"
    "icons/icon32_lingoes.png"
    "icons/icon32_sdict.png"
    "icons/icon32_slob.png"
    "icons/icon32_stardict.png"
    "icons/icon32_wiki.png"
    "icons/icon32_xdxf.png"
    "icons/icon32_zim.png"
    "icons/icon32_zoombase.svg"
    "icons/icon32_zoomin.svg"
    "icons/icon32_zoomout.svg"
    "icons/icon_dsl_native.png"
    "icons/interface.svg"
    "icons/internet.svg"
    "icons/macicon.png"
    "icons/mdict-bg.png"
    "icons/mdict.png"
    "icons/menu_button.svg"
    "icons/network.svg"
    "icons/next.svg"
    "icons/old-arrow.png"
    "icons/old-downarrow.png"
    "icons/ontop.svg"
    "icons/playsound.png"
    "icons/playsound_color.svg"
    "icons/playsound_full.png"
    "icons/prefix.png"
    "icons/previous.svg"
    "icons/print.svg"
    "icons/programicon.png"
    "icons/programicon_old.png"
    "icons/programicon_scan.png"
    "icons/programs.svg"
    "icons/pushpin.svg"
    "icons/reload.png"
    "icons/reload.svg"
    "icons/star.svg"
    "icons/star_blue.svg"
    "icons/system-search.svg"
    "icons/text2speech.svg"
    "icons/transliteration.png"
    "icons/video.png"
    "icons/warning.png"
    "icons/webdict.svg"
    "icons/wiktionary.png"
    "icons/windows-list.svg"
    "icons/wizard-selected.svg"
    "icons/wizard.svg"
    "qt-style-macos.css"
    "qt-style-st-babylon.css"
    "qt-style-st-classic.css"
    "qt-style-st-lingoes-blue.css"
    "qt-style-st-lingoes.css"
    "qt-style-st-lingvo.css"
    "qt-style-st-modern.css"
    "qt-style.css"
    "version.txt"
)

qt_add_resources(goldendict "resources"
    PREFIX
        "/"
    FILES
        ${resources_resource_files}
)
set(scripts_resource_files
    "scripts/gd-builtin.js"
    "scripts/gd-custom.js"
    "scripts/iframe-defer.js"
    "scripts/iframeResizer.contentWindow.min.js"
    "scripts/iframeResizer.min.js"
    "scripts/jquery-3.6.0.slim.min.js"
)

qt_add_resources(goldendict "scripts"
    PREFIX
        "/"
    FILES
        ${scripts_resource_files}
)
set(flags_resource_files
    "flags/ad.png"
    "flags/ae.png"
    "flags/af.png"
    "flags/ag.png"
    "flags/ai.png"
    "flags/al.png"
    "flags/am.png"
    "flags/an.png"
    "flags/ao.png"
    "flags/ar.png"
    "flags/as.png"
    "flags/at.png"
    "flags/au.png"
    "flags/aw.png"
    "flags/ax.png"
    "flags/az.png"
    "flags/ba.png"
    "flags/bb.png"
    "flags/bd.png"
    "flags/be.png"
    "flags/bf.png"
    "flags/bg.png"
    "flags/bh.png"
    "flags/bi.png"
    "flags/bj.png"
    "flags/bm.png"
    "flags/bn.png"
    "flags/bo.png"
    "flags/br.png"
    "flags/bs.png"
    "flags/bt.png"
    "flags/bv.png"
    "flags/bw.png"
    "flags/by.png"
    "flags/bz.png"
    "flags/ca.png"
    "flags/catalonia.png"
    "flags/cc.png"
    "flags/cd.png"
    "flags/cf.png"
    "flags/cg.png"
    "flags/ch.png"
    "flags/ci.png"
    "flags/ck.png"
    "flags/cl.png"
    "flags/cm.png"
    "flags/cn.png"
    "flags/co.png"
    "flags/cr.png"
    "flags/cs.png"
    "flags/cu.png"
    "flags/cv.png"
    "flags/cx.png"
    "flags/cy.png"
    "flags/cz.png"
    "flags/de.png"
    "flags/dj.png"
    "flags/dk.png"
    "flags/dm.png"
    "flags/do.png"
    "flags/dz.png"
    "flags/ec.png"
    "flags/ee.png"
    "flags/eg.png"
    "flags/eh.png"
    "flags/en.png"
    "flags/england.png"
    "flags/eo.png"
    "flags/er.png"
    "flags/es.png"
    "flags/et.png"
    "flags/europeanunion.png"
    "flags/fam.png"
    "flags/fi.png"
    "flags/fj.png"
    "flags/fk.png"
    "flags/fm.png"
    "flags/fo.png"
    "flags/folder.png"
    "flags/folder.svg"
    "flags/fr.png"
    "flags/ga.png"
    "flags/gb.png"
    "flags/gd.png"
    "flags/ge.png"
    "flags/gf.png"
    "flags/gh.png"
    "flags/gi.png"
    "flags/gl.png"
    "flags/gm.png"
    "flags/gn.png"
    "flags/gp.png"
    "flags/gq.png"
    "flags/gr.png"
    "flags/gs.png"
    "flags/gt.png"
    "flags/gu.png"
    "flags/gw.png"
    "flags/gy.png"
    "flags/hk.png"
    "flags/hm.png"
    "flags/hn.png"
    "flags/hr.png"
    "flags/ht.png"
    "flags/hu.png"
    "flags/id.png"
    "flags/ie.png"
    "flags/il.png"
    "flags/in.png"
    "flags/io.png"
    "flags/iq.png"
    "flags/ir.png"
    "flags/is.png"
    "flags/it.png"
    "flags/ja.png"
    "flags/jb.png"
    "flags/jm.png"
    "flags/jo.png"
    "flags/jp.png"
    "flags/ke.png"
    "flags/kg.png"
    "flags/kh.png"
    "flags/ki.png"
    "flags/km.png"
    "flags/kn.png"
    "flags/kp.png"
    "flags/kr.png"
    "flags/kw.png"
    "flags/ky.png"
    "flags/kz.png"
    "flags/la.png"
    "flags/lb.png"
    "flags/lc.png"
    "flags/li.png"
    "flags/lk.png"
    "flags/lr.png"
    "flags/ls.png"
    "flags/lt.png"
    "flags/lu.png"
    "flags/lv.png"
    "flags/ly.png"
    "flags/ma.png"
    "flags/mc.png"
    "flags/md.png"
    "flags/me.png"
    "flags/mg.png"
    "flags/mh.png"
    "flags/mk.png"
    "flags/ml.png"
    "flags/mm.png"
    "flags/mn.png"
    "flags/mo.png"
    "flags/mp.png"
    "flags/mq.png"
    "flags/mr.png"
    "flags/ms.png"
    "flags/mt.png"
    "flags/mu.png"
    "flags/mv.png"
    "flags/mw.png"
    "flags/mx.png"
    "flags/my.png"
    "flags/mz.png"
    "flags/na.png"
    "flags/nc.png"
    "flags/ne.png"
    "flags/nf.png"
    "flags/ng.png"
    "flags/ni.png"
    "flags/nl.png"
    "flags/no.png"
    "flags/np.png"
    "flags/nr.png"
    "flags/nu.png"
    "flags/nz.png"
    "flags/om.png"
    "flags/other.png"
    "flags/pa.png"
    "flags/pe.png"
    "flags/pf.png"
    "flags/pg.png"
    "flags/ph.png"
    "flags/pk.png"
    "flags/pl.png"
    "flags/pm.png"
    "flags/pn.png"
    "flags/pr.png"
    "flags/ps.png"
    "flags/pt.png"
    "flags/pw.png"
    "flags/py.png"
    "flags/qa.png"
    "flags/re.png"
    "flags/ro.png"
    "flags/rs.png"
    "flags/ru.png"
    "flags/rw.png"
    "flags/sa.png"
    "flags/sb.png"
    "flags/sc.png"
    "flags/scotland.png"
    "flags/sd.png"
    "flags/se.png"
    "flags/sg.png"
    "flags/sh.png"
    "flags/si.png"
    "flags/sj.png"
    "flags/sk.png"
    "flags/sl.png"
    "flags/sm.png"
    "flags/sn.png"
    "flags/so.png"
    "flags/sr.png"
    "flags/st.png"
    "flags/sv.png"
    "flags/sy.png"
    "flags/sz.png"
    "flags/tc.png"
    "flags/td.png"
    "flags/tf.png"
    "flags/tg.png"
    "flags/th.png"
    "flags/tj.png"
    "flags/tk.png"
    "flags/tl.png"
    "flags/tm.png"
    "flags/tn.png"
    "flags/to.png"
    "flags/tr.png"
    "flags/tt.png"
    "flags/tv.png"
    "flags/tw.png"
    "flags/tz.png"
    "flags/ua.png"
    "flags/ug.png"
    "flags/uk.png"
    "flags/um.png"
    "flags/us.png"
    "flags/uy.png"
    "flags/uz.png"
    "flags/va.png"
    "flags/vc.png"
    "flags/ve.png"
    "flags/vg.png"
    "flags/vi.png"
    "flags/vn.png"
    "flags/vu.png"
    "flags/wales.png"
    "flags/wf.png"
    "flags/wi.png"
    "flags/wiki.png"
    "flags/ws.png"
    "flags/ye.png"
    "flags/yt.png"
    "flags/za.png"
    "flags/zm.png"
    "flags/zw.png"
)

qt_add_resources(goldendict "flags"
    PREFIX
        "/"
    FILES
        ${flags_resource_files}
)

if(CMAKE_BUILD_TYPE STREQUAL Release)
    target_compile_definitions(goldendict PRIVATE
        NDEBUG
    )
endif()

if((QT_VERSION_MAJOR GREATER 5))
    target_link_libraries(goldendict PRIVATE
        Qt::Core5Compat
        webenginecore
    )
endif()

if(WITH_QTMULTIMEDIA_PLAYER)
    target_compile_definitions(goldendict PRIVATE
        MAKE_QTMULTIMEDIA_PLAYER
    )

    target_link_libraries(goldendict PRIVATE
        Qt::Multimedia
    )
endif()

if(WITH_FFMPEG_PLAYER )
    target_compile_definitions(goldendict PRIVATE
        MAKE_FFMPEG_PLAYER
    )
endif()

if(WITH_use_xapian)
    target_compile_definitions(goldendict PRIVATE
        USE_XAPIAN
    )

    target_link_libraries(goldendict PRIVATE
        xapian
    )
endif()

if(APPLE)
    target_sources(goldendict PUBLIC
        lionsupport.mm
        machotkeywrapper.mm
        macmouseover.hh macmouseover.mm
        speechclient.hh
        speechclient_mac.mm
        texttospeechsource.cc texttospeechsource.hh texttospeechsource.ui
    )

    target_link_libraries(goldendict PRIVATE
        bz2
        lzo2
        ogg
        vorbis
        vorbisfile
        z
    )
endif()

if(WIN32)
    target_sources(goldendict PUBLIC
        guids.c
        sapi.hh
        speechclient.hh
        speechclient_win.cc
        speechhlp.cc speechhlp.hh
        sphelper.hh
        texttospeechsource.cc texttospeechsource.hh texttospeechsource.ui
    )

    target_include_directories(goldendict PRIVATE
        winlibs/include
    )

    target_link_libraries(goldendict PRIVATE
        advapi32
        comdlg32
        ogg
        ole32
        oleaut32
        psapi
        vorbis
        vorbisfile
        wsock32
    )
endif()

if(WIN32 AND win32-msvc_x_)
    target_compile_definitions(goldendict PRIVATE
        _CRT_SECURE_NO_WARNINGS
        __WIN32
    )

    target_link_libraries(goldendict PRIVATE
        # Remove: L${CMAKE_CURRENT_SOURCE_DIR}/winlibs/lib/msvc
        ole32
        sapi
        shell32
        user32
    )

    target_compile_options(goldendict
        /Zc:__cplusplus
        /permissive-
        /std:c++17
        /wd4290
    )
endif()

if(((WIN32) AND (win32-msvc_x_)) AND (QMAKE_TARGET.arch___contains___x86_64))
    target_compile_definitions(goldendict PRIVATE
        NOMINMAX
        __WIN64
    )
endif()

if(Debug AND WIN32 AND win32-msvc_x_)
    target_link_libraries(goldendict PRIVATE
        hunspelld
    )
endif()

if(Release AND WIN32 AND win32-msvc_x_)
    target_link_libraries(goldendict PRIVATE
        Hunspell::Hunspell
    )
endif()

if(WIN32 AND WITH_FFMPEG_PLAYER)
    target_link_libraries(goldendict PRIVATE
        ao
        avcodec
        avformat
        avutil
        swresample
    )
endif()

if(Debug AND WIN32)
    set_target_properties(goldendict PROPERTIES
        WIN32_EXECUTABLE FALSE
    )
endif()

if(Release AND WIN32)
    target_compile_definitions(goldendict PRIVATE
        NO_CONSOLE
    )
endif()

if(NOT WITH_no_macos_universal )
    target_compile_definitions(goldendict PRIVATE
        INCLUDE_LIBRARY_PATH
    )
endif()

if(UNIX AND NOT APPLE)
    target_sources(goldendict PUBLIC
        scanflag.cc scanflag.hh scanflag.ui
    )

    target_compile_definitions(goldendict PRIVATE
        HAVE_X11
        PROGRAM_DATA_DIR=\\\"$${PREFIX}/share/goldendict/\\\"
    )
endif()

if((UNIX AND NOT APPLE) AND ((QT_VERSION_MAJOR LESS 6)))
    target_link_libraries(goldendict PRIVATE
        Qt::X11Extras
    )
endif()

if(UNIX AND NOT APPLE AND NOT arm)
    target_link_libraries(goldendict PRIVATE
        X11
        Xtst
    )
endif()

if(FREEBSD)
    target_link_libraries(goldendict PRIVATE
        execinfo
    )
endif()

if(APPLE AND WITH_FFMPEG_PLAYER)
    target_link_libraries(goldendict PRIVATE
        ao
        avcodec
        avformat
        avutil
        swresample
    )
endif()

if(APPLE AND (NOT WITH_no_macos_universal ))
    target_include_directories(goldendict PRIVATE
        maclibs/include
    )

    target_link_libraries(goldendict PRIVATE
        "-framework AppKit"
        "-framework Carbon"
        # Remove: L${CMAKE_CURRENT_SOURCE_DIR}/maclibs/lib
        Hunspell::Hunspell
    )
endif()

if(APPLE AND WITH_no_macos_universal )
    target_include_directories(goldendict PRIVATE
        /opt/homebrew/include
        /usr/local/include
    )

    target_link_libraries(goldendict PRIVATE
        "-framework AppKit"
        "-framework Carbon"
        # Remove: L/opt/homebrew/lib
        # Remove: L/usr/local/lib
    )
endif()

if(WITH_ZIM)
    target_compile_definitions(goldendict PRIVATE
        MAKE_ZIM_SUPPORT
    )

    target_link_libraries(goldendict PRIVATE
        WrapZSTD::WrapZSTD
        lzma
    )
endif()

if(WITH_EXTRA_TIFF_HANDLER)
    target_compile_definitions(goldendict PRIVATE
        MAKE_EXTRA_TIFF_HANDLER
    )

    target_link_libraries(goldendict PRIVATE
        TIFF::TIFF
    )
endif()

if(NOT WITH_EPWING)
    target_compile_definitions(goldendict PRIVATE
        NO_EPWING_SUPPORT
    )
endif()

if(WITH_EPWING)
    target_sources(goldendict PUBLIC
        epwing.cc epwing.hh
        epwing_book.cc epwing_book.hh
        epwing_charmap.cc epwing_charmap.hh
    )

    target_link_libraries(goldendict PRIVATE
        eb
    )
endif()

if(WITH_OPENCC)
    target_sources(goldendict PUBLIC
        chinese.cc chinese.hh
        chineseconversion.cc chineseconversion.hh chineseconversion.ui
    )

    target_compile_definitions(goldendict PRIVATE
        MAKE_CHINESE_CONVERSION_SUPPORT
    )

    target_link_libraries(goldendict PRIVATE
        opencc
    )
endif()

install(TARGETS goldendict
    BUNDLE DESTINATION .
    RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR}
)

qt_generate_deploy_app_script(
    TARGET goldendict
    FILENAME_VARIABLE deploy_script
    NO_UNSUPPORTED_PLATFORM_ERROR
)
install(SCRIPT ${deploy_script})
