# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

libudev = dependency('libudev', native: native)

hidapi_hidraw_srcs = files(
	'hid.c',
)

hidapi_hidraw = library(
	'hidapi-hidraw',
	hidapi_hidraw_srcs,
	include_directories: hidapi_include,
	dependencies: [threading, libudev],
	version: meson.project_version(),
	install: install_targets,
	native: native,
)

hidapi_hidraw_dep = declare_dependency(
	include_directories: hidapi_include,
	link_with: hidapi_hidraw,
)

meson.override_dependency('hidapi-hidraw', hidapi_hidraw_dep, native: native)

pkgconfig_files = {
	'hidapi-hidraw': hidapi_hidraw
}
