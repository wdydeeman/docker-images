FROM oraclelinux:7-slim

RUN set -eux; \
	yum install -y \
		gzip \
		tar \
		\
# java.lang.UnsatisfiedLinkError: /usr/java/openjdk-12/lib/libfontmanager.so: libfreetype.so.6: cannot open shared object file: No such file or directory
# https://github.com/docker-library/openjdk/pull/235#issuecomment-424466077
		freetype fontconfig \
	; \
	rm -rf /var/cache/yum

# Default to UTF-8 file.encoding
ENV LANG en_US.UTF-8

ENV JAVA_HOME /usr/java/openjdk-14
ENV PATH $JAVA_HOME/bin:$PATH

# https://jdk.java.net/
ENV JAVA_VERSION 14
ENV JAVA_URL https://download.java.net/java/early_access/jpackage/8/openjdk-14-jpackage+8_linux-x64_bin.tar.gz
ENV JAVA_SHA256 1d549f3ba636a97ff236203729b527b7f2f027bf72d034681168eb4d0c2acfb1

RUN set -eux; \
	\
	curl -fL -o /openjdk.tgz "$JAVA_URL"; \
	echo "$JAVA_SHA256 */openjdk.tgz" | sha256sum -c -; \
	mkdir -p "$JAVA_HOME"; \
	tar --extract --file /openjdk.tgz --directory "$JAVA_HOME" --strip-components 1; \
	rm /openjdk.tgz; \
	\
# https://github.com/oracle/docker-images/blob/a56e0d1ed968ff669d2e2ba8a1483d0f3acc80c0/OracleJava/java-8/Dockerfile#L17-L19
	ln -sfT "$JAVA_HOME" /usr/java/default; \
	ln -sfT "$JAVA_HOME" /usr/java/latest; \
	for bin in "$JAVA_HOME/bin/"*; do \
		base="$(basename "$bin")"; \
		[ ! -e "/usr/bin/$base" ]; \
		alternatives --install "/usr/bin/$base" "$base" "$bin" 20000; \
	done; \
	\
# https://github.com/docker-library/openjdk/issues/212#issuecomment-420979840
# https://openjdk.java.net/jeps/341
	java -Xshare:dump; \
	\
# basic smoke test
	java --version; \
	javac --version; \
    jpackage --help



# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]