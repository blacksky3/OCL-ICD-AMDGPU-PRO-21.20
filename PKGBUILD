# _     _            _        _          _____
#| |__ | | __ _  ___| | _____| | ___   _|___ /
#| '_ \| |/ _` |/ __| |/ / __| |/ / | | | |_ \
#| |_) | | (_| | (__|   <\__ \   <| |_| |___) |
#|_.__/|_|\__,_|\___|_|\_\___/_|\_\\__, |____/
#                                  |___/

#Maintainer: blacksky3 <blacksky3@tuta.io> <https://github.com/blacksky3>


#Upstream pages
#https://repo.radeon.com/amdgpu

major=21.20
minor=1271047
ubuntu_ver=20.04

pkgbase=ocl-icd-amdgpu-pro-21.20
pkgname=(ocl-icd-amdgpu-pro-21.20 lib32-ocl-icd-amdgpu-pro-21.20)
pkgver=${major}_${minor}
pkgrel=1
arch=(x86_64)
url='https://repo.radeon.com/amdgpu'
license=(custom: multiple)
makedepends=(wget)
source=(https://github.com/blacksky3/OCL-ICD-AMDGPU-PRO-21.20/releases/download/21.20/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}.tar.xz)

# extracts a debian package
# $1: deb file to extract
extract_deb(){
  local tmpdir="$(basename "${1%.deb}")"
  rm -Rf "$tmpdir"
  mkdir "$tmpdir"
  cd "$tmpdir"
  ar x "$1"
  tar -C "${pkgdir}" -xf data.tar.xz
}

# move ubuntu specific /usr/lib/x86_64-linux-gnu to /usr/lib
# $1: debian package library dir (goes from opt/amdgpu or opt/amdgpu-pro and from x86_64 or i386)
# $2: arch package library dir (goes to usr/lib or usr/lib32)
move_libdir(){
  local deb_libdir="$1"
  local arch_libdir="$2"

  if [ -d "${pkgdir}/${deb_libdir}" ]; then
    if [ ! -d "${pkgdir}/${arch_libdir}" ]; then
      mkdir -p "${pkgdir}/${arch_libdir}"
    fi
    mv -t "${pkgdir}/${arch_libdir}/" "${pkgdir}/${deb_libdir}"/*
    find ${pkgdir} -type d -empty -delete
  fi
}

# move copyright file to proper place and remove debian changelog
move_copyright(){
  find ${pkgdir}/usr/share/doc -name "changelog.Debian.gz" -delete
  mkdir -p ${pkgdir}/usr/share/licenses/${pkgname}
  find ${pkgdir}/usr/share/doc -name "copyright" -exec mv {} ${pkgdir}/usr/share/licenses/${pkgname} \;
  find ${pkgdir}/usr/share/doc -type d -empty -delete
}

package_ocl-icd-amdgpu-pro-21.20(){
  pkgdesc='AMD OpenCL ICD Loader library. Version 21.20'
  arch=(x86_64)
  depends=(glibc)
  optdepends=('opencl-driver: packaged opencl driver')
  conflicts=(libcl ocl-icd ocl-icd-git khronos-ocl-icd khronos-ocl-icd-git)
  replaces=(libcl)
  provides=(opencl-icd-loader ocl-icd ocl-icd-amdgpu-pro)

  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}_amd64.deb
  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}_amd64.deb
  move_libdir "opt/amdgpu-pro/lib/x86_64-linux-gnu" "usr/lib"
  move_copyright

  sed -i "s#prefix=/opt/amdgpu-pro#prefix=/usr#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#libdir=\${prefix}/lib/x86_64-linux-gnu#libdir=\${prefix}/lib#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#L/opt/amdgpu-pro/lib/x86_64-linux-gnu#L/usr/lib#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
  sed -i "s#-I/opt/amdgpu-pro/include#-I/usr/include#g" "${pkgdir}"/usr/lib/pkgconfig/OpenCL.pc
}

package_lib32-ocl-icd-amdgpu-pro-21.20(){
  pkgdesc='AMD OpenCL ICD Loader library. Version 21.20 (32-bit)'
  arch=(i686 x86_64)
  depends=(lib32-glibc ocl-icd-amdgpu-pro-21.20=${major}_${minor}-${pkgrel})
  optdepends=('lib32-opencl-driver: packaged opencl driver')
  conflicts=(lib32-libcl lib32-ocl-icd lib32-ocl-icd-git lib32-khronos-ocl-icd lib32-khronos-ocl-icd-git)
  replaces=(lib32-libcl)
  provides=(lib32-opencl-icd-loader lib32-ocl-icd lib32-ocl-icd-amdgpu-pro)

  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/ocl-icd-libopencl1-amdgpu-pro_${major}-${minor}_i386.deb
  extract_deb "${srcdir}"/amdgpu-pro-${major}-${minor}-ubuntu-${ubuntu_ver}/ocl-icd-libopencl1-amdgpu-pro-dev_${major}-${minor}_i386.deb
  move_libdir "opt/amdgpu-pro/lib/i386-linux-gnu" "usr/lib32"
  move_copyright

  sed -i "s#prefix=/opt/amdgpu-pro#prefix=/usr#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#libdir=\${prefix}/lib/x86_64-linux-gnu#libdir=\${prefix}/lib32#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#L/opt/amdgpu-pro/lib/x86_64-linux-gnu#L/usr/lib32#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
  sed -i "s#-I/opt/amdgpu-pro/include#-I/usr/include#g" "${pkgdir}"/usr/lib32/pkgconfig/OpenCL.pc
}

sha256sums=('8ea051de8c9c6814eb45ce18d102e639bb6edb5786e948b50c5105e3e21978f9')
