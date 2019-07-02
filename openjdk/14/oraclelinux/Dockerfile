FROM oraclelinux:7-slim

RUN set -eux; \
	yum install -y \
		gzip \
		tar \
		\
# java.lang.UnsatisfiedLinkError: /usr/java/openjdk-12/lib/libfontmanager.so: libfreetype.so.6: cannot open shared object file: No such file or directory
# https://github.com/docker-library/openjdk/pull/235#issuecomment-424466077
		freetype fontconfig \
		oracle-epel-release-el7 \
		unzip \
		make \
		yum-utils \
		yum-config-manager --enable *EPEL \
		python36 \
	; \
	rm -rf /var/cache/yum

# Default to UTF-8 file.encoding
ENV LANG en_US.UTF-8

ENV JAVA_HOME /usr/java/openjdk-12
ENV JAVA_HOME_14 /usr/java/openjdk-14
ENV PATH $JAVA_HOME/bin:$PATH
ENV PATH $JAVA_HOME_14/bin:$PATH


# https://jdk.java.net/
ENV JAVA_VERSION 12.0.1
ENV JAVA_URL https://download.java.net/java/GA/jdk12.0.1/69cfe15208a647278a19ef0990eea691/12/GPL/openjdk-12.0.1_linux-x64_bin.tar.gz
ENV JAVA_SHA256 151eb4ec00f82e5e951126f572dc9116104c884d97f91be14ec11e85fc2dd626
# openjdk14

ENV JAVA_URL_14 https://download.java.net/java/early_access/jpackage/8/openjdk-14-jpackage+8_osx-x64_bin.tar.gz
ENV JAVA_SHA256_14 b21f6d5149f960bd78762153b3cd213e985ed089a68d14c86373a51e60d4538e

RUN set -eux; \
	\
	curl -fL -o /openjdk.tgz "$JAVA_URL"; \
	echo "$JAVA_SHA256 */openjdk.tgz" | sha256sum -c -; \
	echo "installing JAVA "${JAVA_VERSION};\
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
# basic smoke test
	java --version; \
	javac --version


# https://docs.oracle.com/javase/10/tools/jshell.htm
# https://docs.oracle.com/javase/10/jshell/
# https://en.wikipedia.org/wiki/JShell
CMD ["jshell"]


