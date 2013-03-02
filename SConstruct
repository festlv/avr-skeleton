from build_utils import read_fuses, write_fuses, upload_hex


#VariantDir allows to separate src and build directories
VariantDir('build','src')

# Create and initialize the environment.
env = Environment()

# Set environment for AVR-GCC.
env['CC'] = 'avr-gcc'
env['CPPPATH'] = ['/usr/avr/include/', 'build']
env['OBJCOPY'] = 'avr-objcopy'
env['SIZE'] = 'avr-size'
env.Append(CCFLAGS = '-Os -Wall -Werror')

# Declare some variables about microcontroller.
# Microcontroller type.
DEVICE = 'atmega2561'
# Microcontroller frequency.
CPU_FREQUENCY = '8000000UL' # Hz


# Set environment for an Atmel AVR Atmega 328p microcontroller.
# Create and initialize the environment.
env.Append(CCFLAGS = '-mmcu=' + DEVICE)
env.Append(CCFLAGS = '-std=c99')
env.Append(LINKFLAGS = '-mmcu=' + DEVICE)
env.Append(LINKFLAGS = '-Wl,-u,vfprintf -lprintf_min')
env.Append(LINKFLAGS = '-lm')
env.Append(CPPDEFINES = 'F_CPU=' + CPU_FREQUENCY)

# Define target name.
TARGET = 'build/main'

# Define source file.
sources = [Glob('build/*.c'), Glob('build/*/*.c')]

# Build the program.
# Default() is used so that when running scons only sources are 
# compiled and linked- no other commands (see below) are run

Default(env.Program(target = TARGET + '.elf', source = sources))

# Create hex binary file.
Default(env.Command(TARGET + '.hex', TARGET + '.elf', env['OBJCOPY'] + ' -O ihex $SOURCE $TARGET'))

# Compute memory usage.
Default(env.Command(None, TARGET + '.elf', env['SIZE'] + ' -C --mcu=' + DEVICE + ' $SOURCE'))
Default(env.Command(None, None, 'ctags -R -f src/.tags src'))


# avrdude settings
env.Append(AVRDUDE_BASE = 'avrdude -c usbasp -p %s ' % (DEVICE))

env.Append(LFUSE = '0xfd')
env.Append(HFUSE = '0x99')
env.Append(EFUSE = '0xff')

# avrdude command wrappers

env.Alias('read-fuses', env.Command('read-fuses.dummy', [], read_fuses))

env.Alias('write-fuses', env.Command('write-fuses.dummy', [], write_fuses))
upload = env.Alias('upload', TARGET+'.hex', upload_hex);
AlwaysBuild(upload)
