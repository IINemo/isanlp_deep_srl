FROM inemo/isanlp_base

RUN apt-get update && apt-get install -y python-pip
RUN curl -s https://packagecloud.io/install/repositories/github/git-lfs/script.deb.sh | bash && \
    apt-get install -y git-lfs

# NLTK
RUN pip2 install nltk
RUN python2 -c "import nltk;\
               nltk.download('punkt')"


# Deep SRl
RUN apt-get update && apt-get install -y tcsh
RUN pip2 install --upgrade pip && pip2 install numpy==1.15.4 theano==0.9.0 protobuf nltk
#COPY deep_srl /src/deep_srl
RUN mkdir /src/deep_srl && cd /src/deep_srl/ && git clone https://github.com/IINemo/deep_srl ./
RUN cd /src/deep_srl/resources && \
    tar -xf conll05_model.tar.gz && \
    tar -xf conll05_propid_model.tar.gz

#COPY glove.6B.100d.txt /src/deep_srl/data/glove/
RUN mkdir -p /src/deep_srl/data/glove/
RUN wget -q http://nlp.stanford.edu/data/glove.6B.zip -O /src/deep_srl/data/glove/glove.6B.zip
RUN apt-get install -y unzip
RUN cd /src/deep_srl/data/glove/ && unzip ./glove.6B.zip

COPY service /src/service

ENV PYTHONPATH=/src/:/src/deep_srl/python

CMD [ "python", "/start.py", "-m", "service.pipeline_object", "-a", "DEEP_SRL" ]
