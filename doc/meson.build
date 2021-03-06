if get_option('gtk_doc')

subdir('xml')

private_headers = [
]

content_files = [
  'overview.xml',
]

glib_prefix = dependency('glib-2.0').get_pkgconfig_variable('prefix')
glib_docpath = join_paths(glib_prefix, 'share', 'gtk-doc', 'html')
docpath = join_paths(get_option('datadir'), 'gtk-doc', 'html')

gnome.gtkdoc('libfeedback',
           main_xml: 'libfeedback-docs.xml',
            src_dir: [
                       join_paths(meson.source_root(), 'libfeedback'),
                       join_paths(meson.build_root(), 'libfeedback'),
		       join_paths(meson.build_root(), 'data'),
                     ],
       dependencies: libfeedback_dep,
  gobject_typesfile: 'libfeedback.types',
          scan_args: [
                       '--rebuild-types',
                       '--ignore-headers=' + ' '.join(private_headers),
                     ],
       fixxref_args: [
                       '--html-dir=@0@'.format(docpath),
                       '--extra-dir=@0@'.format(join_paths(glib_docpath, 'glib')),
                       '--extra-dir=@0@'.format(join_paths(glib_docpath, 'gobject')),
                       '--extra-dir=@0@'.format(join_paths(glib_docpath, 'gio')),
                       '--extra-dir=@0@'.format(join_paths(glib_docpath, 'gi')),
                     ],
        install_dir: 'libfeedback',
      content_files: content_files,
            install: true)
endif

if get_option('man')
  manpages = [
    'fbcli',
    'feedbackd',
  ]

  xsltproc = find_program('xsltproc', required : true)
  xsltproc_command = [
    xsltproc,
    '--nonet',
    '--stringparam', 'man.output.quietly', '1',
    '--stringparam', 'funcsynopsis.style', 'ansi',
    '--stringparam', 'man.th.extra1.suppress', '1',
    '--stringparam', 'man.authors.section.enabled', '0',
    '--stringparam', 'man.copyright.section.enabled', '0',
    '-o', '@OUTPUT@',
    'http://docbook.sourceforge.net/release/xsl/current/manpages/docbook.xsl',
    '@INPUT@',
  ]
  man1_dir = get_option('mandir') + '/man1'

  foreach page : manpages
    custom_target(page + '-man',
      input  : page + '.xml',
      output : page + '.1',
      command: xsltproc_command,
      install: true,
      install_dir: man1_dir)
  endforeach
endif
