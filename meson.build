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

if target == 'darwin'
elif target == 'linux'
elif target == 'netbsd'
elif target == 'windows'
else
	error('The host being compiled for is not supported.')
endif
