# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

if install_targets
	install_headers(
		files('hidapi_darwin.h'),
		subdir: 'hidapi'
	)
endif

frameworks = dependency(
	'appleframeworks',
	modules: ['CoreFoundation', 'IOKit', 'AppKit'],
	native: native
)

hidapi_darwin_srcs = files(
	'hid.c',
)

hidapi_darwin_includes = [
	hidapi_include,
	include_directories('.'),
]

hidapi_darwin = library(
	'hidapi',
	hidapi_darwin_srcs,
	include_directories: hidapi_darwin_includes,
	dependencies: [threading, frameworks],
	version: meson.project_version(),
	install: install_targets,
)

hidapi_darwin_dep = declare_dependency(
	include_directories: hidapi_darwin_includes,
	link_with: hidapi_darwin,
)

hidapi_dep = hidapi_darwin_dep
meson.override_dependency('hidapi_darwin', hidapi_darwin_dep)

pkgconfig_files = {
	'hidapi': hidapi_darwin
}
