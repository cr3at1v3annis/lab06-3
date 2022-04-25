## Laboratory work VI

Код `cmake.yml`

```sh
name: CPack

on:
  push:
    branches:
    - master


jobs:
  Upload_archive_ZIP_files:
    runs-on: ubuntu-latest
    steps:
      - name: Clone repository
        uses: actions/checkout@v2

      - name: Prepare environment
        run: cmake --version

      - name: Build files
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=ARC ..
          cmake --build .
          cpack -G ZIP
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.0.0
          release_name: Release v3.0.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: 
            _build/solver-1.0.0-Linux.zip
          asset_name: artifacts
          asset_content_type: application/arch
          
  Upload_archive_TGZ_files:
    runs-on: ubuntu-latest
    steps:
      - name: Clone repository
        uses: actions/checkout@v2

      - name: Prepare environment
        run: cmake --version

      - name: Build files
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=ARC ..
          cmake --build .
          cpack -G TGZ
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.1.0
          release_name: Release v3.1.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: 
            _build/solver-1.0.0-Linux.tar.gz
          asset_name: artifacts
          asset_content_type: application/arch

  Upload_RPM:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Prepare environment
        run: sudo apt install git

      - name: Build RPM
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=RPM ..
          cmake --build .
          cpack
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.2.0
          release_name: Release v3.2.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: |
            ./_build/solver-1.0.0-Linux.rpm
          asset_name: solver.rpm
          asset_content_type: application/rpm

  Upload_DEB:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - name: Prepare environment
        run: sudo apt install git

      - name: Build DEB
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=DEB ..
          cmake --build .
          cpack
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.3.0
          release_name: Release v3.3.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: |
            ./_build/solver-1.0.0-Linux.deb
          asset_name: solver.deb
          asset_content_type: application/deb

  Upload_DMG:
    runs-on: macos-latest
    steps:
      - uses: actions/checkout@v2

      - name: Build DMG
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=DMG ..
          cmake --build .
          cpack
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.4.0
          release_name: Release v3.4.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: |
            ./_build/solver-1.0.0-Darwin.dmg
          asset_name: solver.dmg
          asset_content_type: application/dmg

  Upload_MSI:
    runs-on: windows-latest
    steps:
      - uses: actions/checkout@v2

      - name: Build MSI
        shell: bash
        run: |
          mkdir _build && cd _build
          cmake -DFlag=MSI ..
          cmake --build .
          cpack -C CPackConfig.cmake
      - name: Create Release
        id: create_release
        uses: actions/create-release@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          tag_name: v3.5.0
          release_name: Release v3.5.0
          draft: false
          prerelease: false

      - name: Upload Release Asset
        id: upload_release_asset
        uses: actions/upload-release-asset@v1
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          upload_url: ${{ steps.create_release.outputs.upload_url }}
          asset_path: |
            ./_build/solver-1.0.0-win64.msi
          asset_name: solver.msi
          asset_content_type: application/msi
```

Код `CpackConfig.cmake`
```sh
include(InstallRequiredSystemLibraries)

set(CPACK_PACKAGE_VERSION_MAJOR 1)
set(CPACK_PACKAGE_VERSION_MINOR 0)
set(CPACK_PACKAGE_VERSION_PATCH 0)

set(CPACK_PACKAGE_NAME "solver")
set(CPACK_PACKAGE_CONTACT ${GITHUB_EMAIL})
set(CPACK_PACKAGE_DESCRIPTION_SUMMARY "Solver packaging")

option(Flag "flag" DEFAULT)
if (${Flag} MATCHES ARC)
    set(CPACK_GENERATOR "TGZ;ZIP")
    install(FILES solver_application/equation.cpp DESTINATION code)
    install(TARGETS formatter_ex solver LIBRARY DESTINATION lib)
endif()
if (${Flag} MATCHES RPM)
    set(CPACK_GENERATOR "RPM")
    install(TARGETS solver DESTINATION bin)
endif()
if (${Flag} MATCHES DEB)
    set(CPACK_DEBIAN_PACKAGE_MAINTAINER "cr3at1v3annis")
    set(CPACK_GENERATOR "DEB")
    install(TARGETS solver DESTINATION bin)
endif()
if (${Flag} MATCHES MSI)
    set(MACOSX_BUNDLE TRUE)
    set(CPACK_GENERATOR "WIX")
    install(TARGETS solver DESTINATION bin)
endif()
if (${Flag} MATCHES DMG)
    set(CPACK_GENERATOR "DragNDrop")
    install(TARGETS solver DESTINATION bin)
endif()
include(CPack)
```


```
Copyright (c) 2015-2021 The ISC Authors
```
