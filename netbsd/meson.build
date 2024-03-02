# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

hidapi_netbsd_srcs = files(
	'hid.c',
)

hidapi_netbsd = library(
	'hidapi-netbsd',
	hidapi_netbsd_srcs,
	include_directories: hidapi_include,
	dependencies: [threading],
	version: meson.project_version(),
	install: install_targets,
)

hidapi_netbsd_dep = declare_dependency(
	include_directories: hidapi_include,
	link_with: hidapi_netbsd,
)

meson.override_dependency('hidapi-netbsd', hidapi_netbsd_dep)

pkgconfig_files = {
	'hidapi-netbsd': hidapi_netbsd
}
