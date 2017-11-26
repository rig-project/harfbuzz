project('harfbuzz', [ 'cpp', 'c' ])

deps = []

if get_option('use_system_libs')
    deps += dependency('freetype2')
    deps += dependency('icu-uc')
else
    subproject('freetype2')
    subproject('icuuc')
endif

inc = include_directories(
        'src',
        'src/hb-ucdn')

public_inc = include_directories('src')

defines = [
    '-D_ALL_SOURCE=1',
    '-D_GNU_SOURCE=1',
    '-DSTDC_HEADERS=1',
    '-DHAVE_ATEXIT=1',
    '-DHAVE_FALLBACK=1',
    '-DHAVE_FONTCONFIG=1',
    '-DHAVE_FREETYPE=1',
    '-DHAVE_GETPAGESIZE=1',
    '-DHAVE_ICU=1',
    '-DHAVE_INTEL_ATOMIC_PRIMITIVES=1',
    '-DHAVE_ISATTY=1',
    '-DHAVE_MMAP=1',
    '-DHAVE_MPROTECT=1',
    '-DHAVE_OT=1',
    '-DHAVE_PTHREAD=1',
    '-DHAVE_PTHREAD_PRIO_INHERIT=1',
    '-DHAVE_SYSCONF=1',
    '-DHAVE_SYS_MMAN_H=1',
    '-DHAVE_UCDN=1',
    '-DHAVE_UNISTD_H=1',
]

src = [
    'src/hb-atomic-private.hh',
    'src/hb-blob.cc',
    'src/hb-buffer-private.hh',
    'src/hb-buffer-serialize.cc',
    'src/hb-buffer.cc',
    'src/hb-cache-private.hh',
    'src/hb-common.cc',
    'src/hb-face-private.hh',
    'src/hb-face.cc',
    'src/hb-font-private.hh',
    'src/hb-font.cc',
    'src/hb-mutex-private.hh',
    'src/hb-object-private.hh',
    'src/hb-open-file-private.hh',
    'src/hb-open-type-private.hh',
    'src/hb-ot-cmap-table.hh',
    'src/hb-ot-glyf-table.hh',
    'src/hb-ot-head-table.hh',
    'src/hb-ot-hhea-table.hh',
    'src/hb-ot-hmtx-table.hh',
    'src/hb-ot-maxp-table.hh',
    'src/hb-ot-name-table.hh',
    'src/hb-ot-tag.cc',
    'src/hb-private.hh',
    'src/hb-set-private.hh',
    'src/hb-set.cc',
    'src/hb-shape.cc',
    'src/hb-shape-plan-private.hh',
    'src/hb-shape-plan.cc',
    'src/hb-shaper-list.hh',
    'src/hb-shaper-impl-private.hh',
    'src/hb-shaper-private.hh',
    'src/hb-shaper.cc',
    'src/hb-unicode-private.hh',
    'src/hb-unicode.cc',
    'src/hb-utf-private.hh',
    'src/hb-warning.cc',

    'src/hb-ot-font.cc',
    'src/hb-ot-layout.cc',
    'src/hb-ot-layout-common-private.hh',
    'src/hb-ot-layout-gdef-table.hh',
    'src/hb-ot-layout-gpos-table.hh',
    'src/hb-ot-layout-gsubgpos-private.hh',
    'src/hb-ot-layout-gsub-table.hh',
    'src/hb-ot-layout-jstf-table.hh',
    'src/hb-ot-layout-private.hh',
    'src/hb-ot-map.cc',
    'src/hb-ot-map-private.hh',
    'src/hb-ot-shape.cc',
    'src/hb-ot-shape-complex-arabic.cc',
    'src/hb-ot-shape-complex-arabic-fallback.hh',
    'src/hb-ot-shape-complex-arabic-private.hh',
    'src/hb-ot-shape-complex-arabic-table.hh',
    'src/hb-ot-shape-complex-arabic-win1256.hh',
    'src/hb-ot-shape-complex-default.cc',
    'src/hb-ot-shape-complex-hangul.cc',
    'src/hb-ot-shape-complex-hebrew.cc',
    'src/hb-ot-shape-complex-indic.cc',
    'src/hb-ot-shape-complex-indic-private.hh',
    'src/hb-ot-shape-complex-indic-table.cc',
    'src/hb-ot-shape-complex-myanmar.cc',
    'src/hb-ot-shape-complex-thai.cc',
    'src/hb-ot-shape-complex-tibetan.cc',
    'src/hb-ot-shape-complex-use.cc',
    'src/hb-ot-shape-complex-use-private.hh',
    'src/hb-ot-shape-complex-use-table.cc',
    'src/hb-ot-shape-complex-private.hh',
    'src/hb-ot-shape-normalize-private.hh',
    'src/hb-ot-shape-normalize.cc',
    'src/hb-ot-shape-fallback-private.hh',
    'src/hb-ot-shape-fallback.cc',
    'src/hb-ot-shape-private.hh',

    'src/hb-fallback-shape.cc',

    'src/hb-ft.cc',
    'src/hb-ft.h',

    'src/hb-icu.cc',
    'src/hb-icu.h',

    'src/hb-ucdn.cc',
    'src/hb-ucdn/ucdn.h',
    'src/hb-ucdn/ucdn.c',
    'src/hb-ucdn/unicodedata_db.h',
]

ragel = find_program('ragel')

gen_hh = generator(ragel,
                   output: '@BASENAME@.hh',
                   arguments: [ '-e', '-F1', '-o', '@OUTPUT@', '@INPUT@' ])
built = gen_hh.process(
    'src/hb-buffer-deserialize-json.rl',
    'src/hb-buffer-deserialize-text.rl',
    'src/hb-ot-shape-complex-indic-machine.rl',
    'src/hb-ot-shape-complex-myanmar-machine.rl',
    'src/hb-ot-shape-complex-use-machine.rl',
)

hb_lib = library('harfbuzz', src, built,
                 cpp_args: defines,
                 c_args: defines,
                 include_directories: inc,
                 dependencies: deps)
hb_dep = declare_dependency(link_with: hb_lib, include_directories: public_inc)
