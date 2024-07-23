# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

project(
	'hidapi',
	'c',
	version: files('VERSION'),
	meson_version: '>=0.58.0',
	license: 'BSD-3-Clause OR GPL-3.0-only',
	default_options: [
		'c_std=gnu99',
		'warning_level=3',
		'b_pie=true',
		'b_lto=true',
		'cpp_eh=s',
		'buildtype=release',
		'b_ndebug=if-release',
	]
)

native = meson.is_cross_build() and get_option('build_native')
if native
	target = build_machine.system()
else
	target = host_machine.system()
endif
cc = meson.get_compiler('c', native: native)
library_mode = get_option('default_library')
build_static = library_mode in ['static', 'both']
build_shared = library_mode in ['shared', 'both']
# Only Windows does not require a threading library here
threading = dependency('threads', required: target != 'windows', native: native)
install_targets = get_option('install_targets')
with_libusb = get_option('with_libusb')

version = meson.project_version()
if get_option('print_version')
	message(f'hidapi: v@version@')
endif

subdir('hidapi')

# Figure out which backend to configure and build
if target == 'windows'
	subdir('windows')
elif target == 'darwin'
	subdir('mac')
elif target == 'linux'
	subdir('linux')
	# If with_libusb is not true and this is Linux, make sure hidapi_dep is hidapi_hidraw_dep
	if not with_libusb
		hidapi_dep = hidapi_hidraw_dep
	endif
# elif target == 'netbsd'
# 	subdir('netbsd')
# 	# If with_libusb is not true and this is NetBSD, make sure hidapi_dep is hidapi_netbsd_dep
# 	if not with_libusb
# 		hidapi_dep = hidapi_netbsd_dep
# 	endif
elif not get_option('with_libusb')
	# If `with_libusb` is false and it's not one of the natively supported platforms, error
	error('The host being compiled for is not supported.')
endif

# If the target is one of Linux, or NetBSD or something not explicitly supported above
if target not in ['windows', 'darwin'] and get_option('with_libusb')
	subdir('libusb')
endif

# Construct pkg-config files for all the targets built
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
