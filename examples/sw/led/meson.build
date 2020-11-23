project('led', 'c')

objcopy = find_program(meson.get_cross_property('prefix') + '-objcopy')
objdump = find_program(meson.get_cross_property('prefix') + '-objdump')
srec_cat = find_program('srec_cat', required: true)

elf = executable('led.elf',
    sources: files('led.c', 'crt0.S'),
    c_args: [ 
      '-march=' + meson.get_cross_property('march'),
      '-mabi=' + meson.get_cross_property('mabi'),
      '-mcmodel=' + meson.get_cross_property('mcmodel'),
      '-static',
      '-Os',
      '-c',
      '-fvisibility=hidden',
      '-fdata-sections',
      '-ffunction-sections',
    ],
    link_args: [
      '-nostdlib',
      '-nostartfiles',
      '-nodefaultlibs',
      '-T' + meson.source_root() / meson.get_cross_property('linker_script'),
      '-Wl,--gc-sections',
    ]
)

bin = custom_target('bin', 
    output: 'led.bin',
    input: elf,
    command: [objcopy, '-O', 'binary', '@INPUT@', '@OUTPUT@'],
)

vmem = custom_target('vmem',
    output: 'led.vmem',
    input: bin,
    command: [srec_cat, '@INPUT@', '-binary', '-offset', '0x0000', '-byte-swap', '4', '-o', '@OUTPUT@', '-vmem'],
)

dis = custom_target('dis',
    output: 'dis',
    input: elf,
    command: [objdump, '-SD', '@INPUT@'],
)
