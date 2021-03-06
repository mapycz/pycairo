python_sources = [
  '__init__.py',
  '__init__.pyi',
  'py.typed',
]

sources = [
  'bufferproxy.c',
  'cairomodule.c',
  'context.c',
  'device.c',
  'enums.c',
  'error.c',
  'font.c',
  'glyph.c',
  'matrix.c',
  'misc.c',
  'path.c',
  'pattern.c',
  'rectangle.c',
  'region.c',
  'surface.c',
  'textcluster.c',
  'textextents.c',
]

foreach python_file : python_sources
  configure_file(
    input: python_file,
    output: python_file,
    configuration: configuration_data(),
  )
endforeach

python_dep = python.dependency()

cairo_dep = dependency('cairo', version: cair_version_req)

python.install_sources(python_sources,
  pure : false,
  subdir : 'cairo'
)

if python.language_version().version_compare('>= 3.0')
  pc_name = 'py3cairo'
else
  pc_name = 'pycairo'
endif

header_file = configure_file(
  input: 'pycairo.h',
  output: pc_name + '.h',
  configuration: configuration_data(),
)

install_headers(
  [header_file],
  subdir: join_paths(python.get_install_dir(), 'cairo', 'include'),
)
install_headers([header_file], subdir: 'pycairo')

pyext = python.extension_module('_cairo', sources,
  dependencies : [python_dep, cairo_dep],
  install: true,
  subdir : 'cairo',
  c_args: pyext_c_args + main_c_args,
)

pkg = import('pkgconfig')

pkg.generate(
  name: pc_name,
  description: 'Python @0@ bindings for cairo'.format(
    python.language_version().split('.')[0]),
  subdirs: 'pycairo',
  requires: ['cairo'],
)

pycairo_dep = declare_dependency(
  version: pycairo_version,
  include_directories: [include_directories('.')],
)
