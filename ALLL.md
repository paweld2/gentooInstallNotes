

 * ERROR: sys-devel/m4-1.4.18-r1::gentoo failed (compile phase):
 *   emake failed

 * If you activate the lookup-hostname hook to look up your hostname
 * using the dns, you need to install net-dns/bind-tools.

 * ERROR: sys-apps/sysvinit-2.93::gentoo failed (compile phase):
 *   emake failed
 *


 * Applying Gentoo Glibc Patchset 2.29-3
 * ERROR: sys-libs/glibc-2.29-r2::gentoo failed (configure phase):
 *   failed to configure glibc
 *
 * ERROR: dev-libs/libgcrypt-1.8.3::gentoo failed (compile phase):
 *   emake failed
 *


 * ERROR: sys-boot/efibootmgr-16::gentoo failed (compile phase):
 *   emake failed
 *

 * To use lsof features in htop(what processes are accessing
 * what files), you must have sys-process/lsof installed.

* bash-completion support requires:
 * 	app-shells/gentoo-bashcomp


 * Messages for package dev-vcs/git-2.21.0:

 * Please read /usr/share/bash-completion/completions/git for Git bash command
 * completion.
 * Please read /usr/share/git/git-prompt.sh for Git bash prompt
 * Note that the prompt bash code is now in that separate script
 * These additional scripts need some dependencies:
 *   git-quiltimport  : dev-util/quilt
 *   git-instaweb     : || ( www-servers/lighttpd www-servers/apache www-servers/nginx )






 * ERROR: net-wireless/crda-3.18-r3::gentoo failed (compile phase):
 *   emake failed
 *
 * If you need support, post the output of `emerge --info '=net-wireless/crda-3.18-r3::gentoo'`,
 * the complete build log and the output of `emerge -pqv '=net-wireless/crda-3.18-r3::gentoo'`.
 * The complete build log is located at '/var/tmp/portage/net-wireless/crda-3.18-r3/temp/build.log'.
 * The ebuild environment file is located at '/var/tmp/portage/net-wireless/crda-3.18-r3/temp/environment'.
 * Working directory: '/var/tmp/portage/net-wireless/crda-3.18-r3/work/crda-3.18'
 * S: '/var/tmp/portage/net-wireless/crda-3.18-r3/work/crda-3.18'



 * ERROR: sys-fs/inotify-tools-3.20.1::gentoo failed (compile phase):
 *   emake failed
 *
 * If you need support, post the output of `emerge --info '=sys-fs/inotify-tools-3.20.1::gentoo'`,
 * the complete build log and the output of `emerge -pqv '=sys-fs/inotify-tools-3.20.1::gentoo'`.
 * The complete build log is located at '/var/tmp/portage/sys-fs/inotify-tools-3.20.1/temp/build.log'.
 * The ebuild environment file is located at '/var/tmp/portage/sys-fs/inotify-tools-3.20.1/temp/environment'.
 * Working directory: '/var/tmp/portage/sys-fs/inotify-tools-3.20.1/work/inotify-tools-3.20.1'
 * S: '/var/tmp/portage/sys-fs/inotify-tools-3.20.1/work/inotify-tools-3.20.1'


 * ERROR: dev-libs/elfutils-0.173-r1::gentoo failed (configure phase):
 *   econf failed
 *
 * Call stack:
 *               ebuild.sh, line  124:  Called src_configure
 *             environment, line 2740:  Called multilib-minimal_src_configure
 *             environment, line 1940:  Called multilib_foreach_abi 'multilib-minimal_abi_src_configure'
 *             environment, line 2167:  Called multibuild_foreach_variant '_multilib_multibuild_wrapper' 'multilib-minimal_abi_src_configure'
 *             environment, line 1870:  Called _multibuild_run '_multilib_multibuild_wrapper' 'multilib-minimal_abi_src_configure'
 *             environment, line 1868:  Called _multilib_multibuild_wrapper 'multilib-minimal_abi_src_configure'
 *             environment, line  391:  Called multilib-minimal_abi_src_configure
 *             environment, line 1934:  Called multilib_src_configure
 *             environment, line 2387:  Called econf '--enable-nls' '--disable-thread-safety' '--program-prefix=eu-' '--with-zlib' '--with-bzlib' '--without-lzma'
 *        phase-helpers.sh, line  718:  Called __helpers_die 'econf failed'
 *   isolated-functions.sh, line  119:  Called die
 * The specific snippet of code:
 *   		die "$@"


 * ERROR: -1.5.2-r3::gentoo failed (compile phase):
 *   emake failed
 *
 * ERROR: -1.3.2-r8::gentoo failed (compile phase):
 *   emake failed
 *

 * ERROR: -2.0-r1::gentoo failed (compile phase):
 *   emake failed
 *

 * ERROR: -2.02-r3::gentoo failed (compile phase):
 *   emake failed
 *


 * ERROR: -0.2.5-r1::gentoo failed (compile phase):
 *   emake failed

flaggie sys-apps/sysvinit +compiler-gcc

flaggie app-text/openjade +compiler-gcc
flaggie media-gfx/gpicview +compiler-gcc
flaggie sys-boot/grub +compiler-gcc
flaggie mail-mta/nullmailer +compiler-gcc
flaggie app-text/opensp +compiler-gcc
flaggie sys-auth/polkit +compiler-gcc


 * # rc-update add laptop_mode default
 * # rc-update add acpid default

 * Messages for package sys-kernel/dracut-048-r1:



 *   app-admin/syslog-ng for Enable logging with syslog-ng or rsyslog
 *   app-admin/rsyslog for Enable logging with syslog-ng or rsyslog

 * ERROR: -0.115-r3::gentoo failed (compile phase):
 *   emake failed

 * The following 26 packages have failed to build, install, or execute
 * postinst:
 *
emerge -av1 sys-apps/sysvinit sys-libs/glibc dev-libs/libgcrypt sys-boot/efibootmgr net-wireless/crda net-wireless/wpa_supplicant media-fonts/ipamonafont sys-fs/inotify-tools dev-libs/elfutils app-text/opensp app-text/openjade mail-mta/nullmailer sys-boot/grub media-gfx/gpicview lxde-base/lxde-meta sys-auth/polkit sys-auth/consolekit sys-auth/pambase sys-fs/udisks gnome-base/gvfs x11-libs/libfm lxde-base/lxpanel x11-misc/pcmanfm lxde-base/lxsession

emerge -av1 -j8 sys-auth/pambase sys-fs/udisks gnome-base/gvfs x11-libs/libfm lxde-base/lxde-meta x11-misc/pcmanfm lxde-base/lxpanel lxde-base/lxsession