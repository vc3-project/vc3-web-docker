FROM centos:centos7
LABEL maintainer lincolnb@uchicago.edu
LABEL version 2017.9.8-1

RUN yum install epel-release -y 
RUN yum install git python-pip nginx python-devel supervisor pip uwsgi uwsgi-plugin-python -y 
RUN yum groupinstall "Development Tools" -y

RUN mkdir -p /srv/www
RUN git clone https://github.com/vc3-project/vc3-website-python /srv/www/vc3-web-env
RUN cd /srv/www/vc3-web-env; pip install -r requirements.txt

# VC3 components
RUN cd /usr/local

RUN git clone https://github.com/vc3-project/vc3-info-service.git && \
    pushd vc3-info-service && \
    python setup.py install && \
    popd

RUN git clone https://github.com/vc3-project/vc3-client.git && \
    pushd vc3-client && \
    python setup.py install && \
    popd

RUN git clone https://github.com/vc3-project/credible.git && \
    pushd vc3-client && \
    python setup.py install && \
    popd

# Config files
COPY vc3.ini /etc/uwsgi.d/vc3.ini
COPY vc3-client.conf /etc/vc3/vc3-client.conf
COPY uwsgi_params /etc/nginx/uwsgi_params
COPY nginx.conf /etc/nginx/nginx.conf
COPY supervisord.conf /etc/supervisord.conf

CMD ["/usr/bin/supervisord","-c","/etc/supervisord.conf"]
