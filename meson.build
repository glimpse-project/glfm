project('glfm', [ 'c' ],
        license: ['zlib'],
        default_options: ['c_std=c99'])

version='0.9.0'

compiler = meson.get_compiler('c')

if compiler.get_define('__ENVIRONMENT_IPHONE_OS_VERSION_MIN_REQUIRED__') != ''
    add_languages('objc')
endif

libglfm_src = []
libglfm_deps = []
libglfm_cargs = []
libglfm_linkargs = []

if compiler.get_define('__ANDROID__') != ''

pymodule = import('python3')
python3 = pymodule.find_python()
android_ndk_home = run_command(python3, '-c',
    'import os; print(os.getenv("ANDROID_NDK_HOME", ""), end = "")').stdout()

libglfm_src += [
  'src/glfm_platform_android.c',
  android_ndk_home + '/sources/android/native_app_glue/android_native_app_glue.c'
]
libglfm_cargs += [
  '-I' + android_ndk_home + '/sources/android/native_app_glue/'
]
libglfm_linkargs += [
  '-llog',
  '-landroid',
  '-lEGL',
  '-lGLESv2'
]

elif compiler.get_define('__ENVIRONMENT_IPHONE_OS_VERSION_MIN_REQUIRED__') != ''

libglfm_src += [
  'src/glfm_platform_ios.m',
]

else

error('No compatible platform detected')

endif

glfm_lib = static_library('glfm', libglfm_src,
                          version: version,
                          include_directories: include_directories('include'),
                          dependencies: libglfm_deps,
                          c_args: libglfm_cargs,
                          link_args: libglfm_linkargs
)
glfm_dep = declare_dependency(link_with: glfm_lib,
                              include_directories: include_directories('include'),
                              link_args: libglfm_linkargs
)
