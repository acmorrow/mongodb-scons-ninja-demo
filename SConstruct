# -*- mode: python; -*-

DefaultEnvironment(tools=[])

EnsurePythonVersion(3, 6)
EnsureSConsVersion(3, 1, 1)

AddOption(
    "--ninja",
    dest="ninja",
    nargs=0,
    help='Enable the build.ninja generator tool stable or canary version',
)

def variable_shlex_converter(val):
    if not isinstance(val, basestring):
        return val
    return shlex.split(val, posix=True)

env_vars = Variables()

env_vars.Add('BUILD_DIR',
    default='#/build'
)

env_vars.Add('CC')

env_vars.Add('CXX')

env_vars.Add('CCFLAGS', converter=variable_shlex_converter)

env_vars.Add('CXXFLAGS', converter=variable_shlex_converter)

env_vars.Add('NINJA_PREFIX',
    default="build",
)

env_vars.Add('NINJA_SUFFIX')

env_vars.Add('__NINJA_NO',
    help="Disable the Ninja tool unconditionally. Not intended for human use.",
    default=0)


env = Environment(
    variables=env_vars,
    VARIANT_DIR='$BUILD_DIR/variant'
)

sconsDir = env.Dir(env.subst('$BUILD_DIR/scons'))
env.SConsignFile(str(sconsDir.File('signatures')))

if GetOption('ninja') is not None:
    ninja_builder = Tool("ninja_next")
    ninja_builder.generate(env)

env.SConscript(
    dirs=[
        '.'
    ],
    variant_dir=env.subst('$VARIANT_DIR'),
    exports=[
        'env'
    ],
    duplicate=False,
)
