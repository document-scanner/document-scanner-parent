FROM ubuntu:18.04
  # - use `debian` rather than `maven:3.5.2` because the files copied from `/usr/share/maven/ref/` to `/root/.m2/` (see https://hub.docker.com/_/maven/ for a detailed explanation) are only present randomly(!) when executing `docker run`

RUN apt-get update && apt-get install --yes git openjdk-8-jdk openjfx maven wget fakeroot genisoimage gcc make sudo && apt-get clean && rm -rf /var/lib/apt/lists/*
  # `gcc` and `make` are necessary for integration tests using a custom postgresql build through jhbuild-java-wrapper
ENV JAVA_HOME /usr/lib/jvm/java-8-openjdk-amd64/
RUN useradd -ms /bin/bash document-scanner && sudo -u document-scanner -H -E bash -c 'cd $HOME && chown -R document-scanner .'
RUN sudo -u document-scanner -H -E bash -c 'cd $HOME && git clone --branch=types https://github.com/krichter722/JDatePicker.git && cd JDatePicker && env _JAVA_OPTIONS=-Djdk.net.URLClassPath.disableClassPathURLCheck=true mvn --batch-mode install && cd ..'
  # `_JAVA_OPTIONS=-Djdk.net.URLClassPath.disableClassPathURLCheck=true` works around Error: Could not find or load main class org.apache.maven.surefire.booter.ForkedBooter`
RUN sudo -u document-scanner -H -E bash -c 'cd $HOME && git clone https://github.com/opencollab/flexdock.git && cd flexdock && env _JAVA_OPTIONS=-Djdk.net.URLClassPath.disableClassPathURLCheck=true mvn --batch-mode install && cd ..'

ENV NLP_VERSION 3.8.0
RUN sudo -u document-scanner -H -E bash -c 'mkdir -p $HOME/.m2/repository/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/ && \
cd $HOME/.m2/repository/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/ && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-arabic.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-chinese.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-english.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-french.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-german.jar && \
wget -nv https://repo.maven.apache.org/maven2/edu/stanford/nlp/stanford-corenlp/$NLP_VERSION/stanford-corenlp-$NLP_VERSION-models-spanish.jar && \
cd /'
