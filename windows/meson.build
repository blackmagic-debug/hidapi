# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

windows = import('windows')

if install_targets
	install_headers(
		files('hidapi_winapi.h'),
		subdir: 'hidapi'
	)
endif

hidapi_winapi_headers = files(
	'hidapi_cfgmgr32.h',
	'hidapi_descriptor_reconstruct.h',
	'hidapi_hidclass.h',
	'hidapi_hidpi.h',
	'hidapi_hidsdi.h',
)

hidapi_winapi_srcs = files(
	'hid.c',
	'hidapi_descriptor_reconstruct.c',
)

hidapi_winapi_includes = [
	hidapi_include,
	include_directories('.'),
]

hidapi_winapi_rc = windows.compile_resources(
	'hidapi.rc',
	include_directories: hidapi_winapi_includes,
)

if build_static
	hidapi_winapi_static = static_library(
		'hidapi',
		hidapi_winapi_srcs,
		extra_files: hidapi_winapi_headers,
		# prevent marking functions as __declspec(dllexport) for static library build
		# #480: this should be refactored for v1.0
		c_args: ['-DHID_API_NO_EXPORT_DEFINE'],
		include_directories: hidapi_winapi_includes,
		install: install_targets,
		native: native,
	)

	if not build_shared
		hidapi_winapi_dep = declare_dependency(
			include_directories: hidapi_winapi_includes,
			link_with: hidapi_winapi_static,
		)
	endif
endif

if build_shared
	hidapi_winapi_shared = shared_library(
		'hidapi',
		hidapi_winapi_srcs + hidapi_winapi_rc,
		extra_files: hidapi_winapi_headers,
		c_args: ['-Dhidapi_winapi_EXPORTS'],
		include_directories: hidapi_winapi_includes,
		version: meson.project_version(),
		install: install_targets,
		native: native,
	)

	hidapi_winapi_dep = declare_dependency(
		include_directories: hidapi_winapi_includes,
		link_with: hidapi_winapi_shared,
	)
endif

hidapi_dep = hidapi_winapi_dep
meson.override_dependency('hidapi_winapi', hidapi_winapi_dep, native: native)

if build_shared
	pkgconfig_files = {
		'hidapi': hidapi_winapi_shared
	}
else
	pkgconfig_files = {
		'hidapi': hidapi_winapi_static
	}
endif

if get_option('tests')
	subdir('test')
endif
subdir('pp_data_dump')
