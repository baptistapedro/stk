FROM fuzzers/afl:2.52

RUN apt-get update
RUN apt install -y build-essential wget git clang cmake  automake autotools-dev  libtool zlib1g zlib1g-dev libexif-dev  libjpeg-dev libasound-dev
RUN  wget http://ccrma.stanford.edu/software/stk/release/stk-4.6.2.tar.gz
RUN tar xvfz stk-4.6.2.tar.gz
WORKDIR /stk-4.6.2
RUN ./configure CC=afl-clang CXX=afl-clang++
RUN make
RUN make install
COPY file:2010ee9296b52f1c4785b75721d525f7dc94b464450829ac7f51fb196bba43bf in . 
RUN afl-clang++ -I/usr/local/include/stk -L/usr/local/lib fuzz.cpp -o /rawFuzz -lstk
RUN mkdir /rawCorpus
RUN cp ./rawwaves/*.raw /rawCorpus
ENV LD_LIBRARY_PATH=/usr/local/lib/

ENTRYPOINT ["afl-fuzz", "-i", "/rawCorpus", "-o", "/rawOut"]
CMD ["/rawFuzz", "@@"]
