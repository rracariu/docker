FROM rracariu/ldc-linux-x86-64

ARG MSVC_LIBS_URL

RUN apt update && apt install p7zip curl xz-utils -y

RUN curl -L -o ldc2-windows.7z "https://github.com/ldc-developers/ldc/releases/download/v${COMPILER_VERSION}/ldc2-${COMPILER_VERSION}-windows-x64.7z"
RUN 7zr x ldc2-windows.7z
RUN cp ldc2-"${COMPILER_VERSION}"-windows-x64/lib/*.lib "/dlang/ldc-${COMPILER_VERSION}/lib"
RUN sed -i -e 's/        "-L--no-warn-search-mismatch",/        "-mtriple=x86_64-pc-windows-msvc",\n        "-link-internally",/g' "/dlang/ldc-${COMPILER_VERSION}/etc/ldc2.conf"

RUN curl -L -o libs_msvc.tar.xz "${MSVC_LIBS_URL}"
RUN mkdir libs_msvc && tar xf libs_msvc.tar.xz -C libs_msvc
RUN cd libs_msvc && cp \
  advapi32.lib \
  comdlg32.lib \
  gdi32.lib \
  kernel32.lib \
  legacy_stdio_definitions.lib \
  legacy_stdio_wide_specifiers.lib \
  libcmt.lib \
  libucrt.lib \
  libvcruntime.lib \
  oldnames.lib \
  ole32.lib \
  oleaut32.lib \
  shell32.lib \
  user32.lib \
  uuid.lib \
  winspool.lib \
  ws2_32.lib \
  wsock32.lib \
  /dlang/ldc-${COMPILER_VERSION}/lib && \
  ln -s "/dlang/ldc-${COMPILER_VERSION}/lib/oldnames.lib" "/dlang/ldc-${COMPILER_VERSION}/lib/OLDNAMES.lib"

RUN rm -rf \
  "ldc2-${COMPILER_VERSION}-windows-x64" \
  ldc2-windows.7z \
  libs_msvc \
  libs_msvc.tar.xz \
  /var/lib/apt/lists/*

COPY entrypoint.sh /
RUN chmod +x /entrypoint.sh