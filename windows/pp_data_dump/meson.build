# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

pp_data_dump = executable(
	'pp_data_dump',
	'pp_data_dump.c',
	override_options: [
		'c_std=c11',
	],
	dependencies: [hidapi_winapi_dep],
	install: install_targets,
	native: native,
)
