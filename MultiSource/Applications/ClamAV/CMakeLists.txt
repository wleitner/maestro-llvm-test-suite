list(APPEND CPPFLAGS -DHAVE_CONFIG_H -I${CMAKE_CURRENT_SOURCE_DIR} -I${CMAKE_CURRENT_SOURCE_DIR}/zlib -DDONT_LOCK_DBDIRS)

set(INPUT
  inputs/clam.cab
  inputs/clamdoc.tar.gz
  inputs/clam.exe
  inputs/clam.exe.bz2
  inputs/clam-v2.rar
  inputs/clam-v3.rar
  inputs/clam.zip
  inputs/README
  inputs/rtf-test/Doc11.rtf
  inputs/rtf-test/Doc1.rtf
  inputs/rtf-test/Doc22.rtf
  inputs/rtf-test/Doc2.rtf
  inputs/rtf-test/doc3.rtf
  inputs/rtf-test/docCLAMexe.rtf
  inputs/rtf-test/rtf1.rtf
  inputs/rtf-test/rtf-novirus.rtf
)
set(RUN_OPTIONS --debug --exclude-dir .svn --verbose -d%S/dbdir -r ${INPUT})
if(TARGET_OS STREQUAL "Darwin")
  list(APPEND CPPFLAGS -DC_DARWIN)
endif()
if(TARGET_OS STREQUAL "AIX")
  list(APPEND CPPFLAGS -DC_AIX)
endif()
if(TARGET_OS STREQUAL "IRIX")
  list(APPEND CPPFLAGS -DC_IRIX)
endif()
if(TARGET_OS STREQUAL "Cygwin")
  list(APPEND CPPFLAGS -DC_CYGWIN)
endif()
if(TARGET_OS STREQUAL "FreeBSD")
  list(APPEND CPPFLAGS -DC_BSD)
endif()
if(TARGET_OS STREQUAL "OpenBSD")
  list(APPEND CPPFLAGS -DC_BSD)
endif()
if(TARGET_OS STREQUAL "NetBSD")
  list(APPEND CPPFLAGS -DC_BSD)
endif()
if(TARGET_OS STREQUAL "HP-UX")
  list(APPEND CPPFLAGS -DC_HPUX)
endif()
if(TARGET_OS STREQUAL "Interix")
  list(APPEND CPPFLAGS -DC_INTERIX -DC_WINDOWS)
endif()
if(TARGET_OS STREQUAL "Win32")
  list(APPEND CPPFLAGS -DC_WINDOWS)
endif()
if(TARGET_OS STREQUAL "mingw")
  list(APPEND CPPFLAGS -DC_WINDOWS)
endif()
if(TARGET_OS STREQUAL "Linux")
  list(APPEND CPPFLAGS -DC_LINUX)
endif()
if(NOT ARCH STREQUAL "ARM")
  if(ENDIAN STREQUAL "little")
    list(APPEND CPPFLAGS -DFPU_WORDS_BIGENDIAN=0)
  endif()
  if(ENDIAN STREQUAL "big")
    list(APPEND CPPFLAGS -DFPU_WORDS_BIGENDIAN=1)
  endif()
endif()
if(ENDIAN STREQUAL "little")
  list(APPEND CPPFLAGS -DWORDS_BIGENDIAN=0)
endif()
if(ENDIAN STREQUAL "big")
  list(APPEND CPPFLAGS -DWORDS_BIGENDIAN=1)
endif()
llvm_multisource(clamscan
  clamscan_clamscan.c
  clamscan_manager.c
  clamscan_others.c
  clamscan_treewalk.c
  libclamav_aspack.c
  libclamav_autoit.c
  libclamav_binhex.c
  libclamav_blob.c
  libclamav_cab.c
  libclamav_chmunpack.c
  libclamav_cvd.c
  libclamav_dconf.c
  libclamav_dsig.c
  libclamav_elf.c
  libclamav_entconv.c
  libclamav_filetypes.c
  libclamav_fsg.c
  libclamav_hashtab.c
  libclamav_htmlnorm.c
  libclamav_is_tar.c
  libclamav_jscript.c
  libclamav_line.c
  libclamav_lockdb.c
  libclamav_matcher-ac.c
  libclamav_matcher-bm.c
  libclamav_matcher.c
  libclamav_mbox.c
  libclamav_md5.c
  libclamav_message.c
  libclamav_mew.c
  libclamav_msexpand.c
  libclamav_mspack.c
  libclamav_nsis_LZMADecode.c
  libclamav_nsis_bzlib.c
  libclamav_nsis_infblock.c
  libclamav_nsis_nulsft.c
  libclamav_ole2_extract.c
  libclamav_others.c
  libclamav_packlibs.c
  libclamav_pdf.c
  libclamav_pe.c
  libclamav_petite.c
  libclamav_phish_domaincheck_db.c
  libclamav_phish_whitelist.c
  libclamav_phishcheck.c
  libclamav_pst.c
  libclamav_readdb.c
  libclamav_rebuildpe.c
  libclamav_regex_list.c
  libclamav_regex_regcomp.c
  libclamav_regex_regerror.c
  libclamav_regex_regexec.c
  libclamav_regex_regfree.c
  libclamav_regex_strlcpy.c
  libclamav_rtf.c
  libclamav_scanners.c
  libclamav_sis.c
  libclamav_snprintf.c
  libclamav_special.c
  libclamav_spin.c
  libclamav_str.c
  libclamav_suecrypt.c
  libclamav_table.c
  libclamav_text.c
  libclamav_tnef.c
  libclamav_unarj.c
  libclamav_unsp.c
  libclamav_untar.c
  libclamav_unzip.c
  libclamav_upack.c
  libclamav_upx.c
  libclamav_uuencode.c
  libclamav_vba_extract.c
  libclamav_wwunpack.c
  libclamav_yc.c
  shared_cdiff.c
  shared_cfgparser.c
  shared_getopt.c
  shared_misc.c
  shared_network.c
  shared_options.c
  shared_output.c
  shared_sha256.c

  zlib_adler32.c
  zlib_compress.c
  zlib_crc32.c
  zlib_deflate.c
  zlib_gzclose.c
  zlib_gzlib.c
  zlib_gzread.c
  zlib_gzwrite.c
  zlib_infback.c
  zlib_inffast.c
  zlib_inflate.c
  zlib_inftrees.c
  zlib_trees.c
  zlib_uncompr.c
  zlib_zutil.c
)
if(TARGET_OS STREQUAL "Linux")
  find_package(Intl REQUIRED)
  target_link_libraries(clamscan ${Intl_LIBRARIES})
endif()
llvm_test_data(clamscan ${INPUT} dbdir)
