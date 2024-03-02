# SPDX-License-Identifier: BSD-3-Clause
# SPDX-FileCopyrightText: 2023 Rachel Mant <git@dragonmux.network>

if install_targets
	install_headers(
		files('hidapi_libusb.h'),
		subdir: 'hidapi'
	)
endif

libusb = dependency(
	'libusb-1.0',
	version: '>=1.0.9',
	method: 'pkg-config',
)

hidapi_libusb_defines = []
with_iconv = get_option('iconv')
iconv_test = '''
	#include <stddef.h>
	#include <iconv.h>
	int main() {
		char *a, *b;
		size_t i, j;
		iconv_t ic;
		ic = iconv_open(\"to\", \"from\");
		iconv(ic, &a, &i, &b, &j);
		iconv_close(ic);
	}
'''

# Try to figure out if iconv is enabled and supported
iconv = declare_dependency()
if with_iconv.disabled()
	hidapi_libusb_defines += ['-DNO_ICONV']
# If it is not explicitly disabled, we are not on Android, and the compiler is not providing iconv support
elif target != 'android' and not cc.links(iconv_test)
	# Try to find libiconv via pkg-config
	iconv = dependency('iconv', required: false)
	# Check if what was found works
	if not cc.links(iconv_test, dependencies: iconv)
		# If it does not, try to find iconv via the compiler
		iconv = cc.find_library('iconv', required: false)
		# Check if that works
		if not cc.links(iconv_test, dependencies: iconv)
			# If we found no working iconv's, fall back to having iconv support disabled
			hidapi_libusb_defines += ['-DNO_ICONV']
			iconv = declare_dependency()
		endif
	endif
endif

hidapi_libusb_srcs = files(
	'hid.c',
)

hidapi_libusb_includes = [
	hidapi_include,
	include_directories('.'),
]

hidapi_libusb = library(
	'hidapi-libusb',
	hidapi_libusb_srcs,
	c_args: hidapi_libusb_defines,
	include_directories: hidapi_libusb_includes,
	dependencies: [threading, libusb, iconv],
	version: meson.project_version(),
	install: install_targets,
)

hidapi_libusb_dep = declare_dependency(
	include_directories: hidapi_libusb_includes,
	link_with: hidapi_libusb,
)

hidapi_dep = hidapi_libusb_dep
meson.override_dependency('hidapi-libusb', hidapi_libusb_dep)

pkgconfig_files += {
	'hidapi-libusb': hidapi_libusb
}
