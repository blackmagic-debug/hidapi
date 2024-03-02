# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

project(
	'hidapi',
	'c',
	version: files('VERSION'),
	meson_version: '>=0.58.0',
	license: 'BSD-3-Clause OR GPL-3.0-only',
	default_options: [
		'warning_level=3',
		'b_pie=true',
		'b_lto=true',
		'cpp_eh=s',
		'buildtype=release',
		'b_ndebug=if-release',
	]
)

target = target_machine.system()
cc = meson.get_compiler('c')
library_mode = get_option('default_library')
build_static = library_mode in ['static', 'both']
build_shared = library_mode in ['shared', 'both']
threading = dependency('threads', required: target != 'windows')

version = meson.project_version()
if get_option('print_version')
	message(f'hidapi: v@version@')
endif

install_targets = get_option('install_targets')

subdir('hidapi')

if target == 'windows'
	subdir('windows')
elif target == 'darwin'
	subdir('mac')
elif target == 'linux'
	subdir('linux')
endif

if target == 'darwin'
elif target == 'linux'
elif target == 'netbsd'
elif target == 'windows'
else
	error('The host being compiled for is not supported.')
endif

pkgconfig = import('pkgconfig')
foreach name, lib : pkgconfig_files
	pkgconfig.generate(
		lib,
		name: name,
		filebase: name,
		subdirs: 'hidapi',
		url: 'https://github.com/libusb/hidapi',
		description: 'C Library for USB/Bluetooth HID device access from Linux, Mac OS X, FreeBSD, and Windows.',
	)
endforeach

meson.override_dependency(meson.project_name(), hidapi_dep)
