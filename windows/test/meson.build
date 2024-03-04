# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

reconstructor_test = executable(
	'hid_report_reconstructor_test',
	'hid_report_reconstructor_test.c',
	override_options: [
		'c_std=c11',
	],
	dependencies: [hidapi_winapi_dep],
	install: false,
	native: native,
)

# Each test case requires 2 files:
# <name>.pp_data - textual representation of HIDP_PREPARSED_DATA;
# <name>_expected.rpt_desc - reconstructed HID Report Descriptor out of .pp_data file;
#
# (Non-required by test):
# <name>_real.dpt_desc - the original report rescriptor used to create a test case;
reconstruct_test_cases = [
	'046D_C52F_0001_000C',
	'046D_C52F_0001_FF00',
	'046D_C52F_0002_0001',
	'046D_C52F_0002_FF00',
	'17CC_1130_0000_FF01',
	'046D_0A37_0001_000C',
	'046A_0011_0006_0001',
	'046D_C077_0002_0001',
	'046D_C283_0004_0001',
	'046D_B010_0006_0001',
	'046D_B010_0002_FF00',
	'046D_B010_0002_0001',
	'046D_B010_0001_FF00',
	'046D_B010_0001_000C',
	'046D_C534_0001_000C',
	'046D_C534_0001_FF00',
	'046D_C534_0002_0001',
	'046D_C534_0002_FF00',
	'046D_C534_0006_0001',
	'046D_C534_0080_0001',
	'047F_C056_0001_000C',
	'047F_C056_0003_FFA0',
	'047F_C056_0005_000B',
	'045E_02FF_0005_0001',
]

foreach case : reconstruct_test_cases
	pp_data = files(f'@case@.pp_data')
	expected_descriptor = files(f'@case@_expected.rpt_desc')

	test(
		f'WinHidReportReconstructTest_@case@',
		reconstructor_test,
		args: [pp_data, expected_descriptor],
		workdir: meson.current_build_dir() / '..',
	)
endforeach
