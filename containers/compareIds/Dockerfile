# Choose any base image you prefer, like bioconda, ubuntu, alpine, etc..
# or using a multistage deployment
FROM centos:7.6.1810

# Use a single LABEL (multiple ones are discouraged because each one creates a new layer)
LABEL	description="Sample file validator" vendor="ISCIII" version="0.1" maintainer="Sara Monzon <smonzon@isciii.es>"

COPY ./scif_app_recipes /opt/scif_app_recipes
COPY ./containers/compareIds/compareIds.py /opt/

# First, install all the dependencies needed by the results checking procedure
RUN echo "Install basic development tools" && \
    yum -y groupinstall "Development Tools" && \
    yum -y update && yum -y install wget curl && \
    echo "Install python2.7 setuptools and pip" && \
    yum -y install python-setuptools && \
    easy_install pip && \
    echo "Installing SCI-F" && \
    pip install scif==0.0.77 ipython


# Now, we get the validation program in some way (copying, git clone, etc...)
# which should also write the results in the canonical format expected by the
# community metrics docker instances.
# The return value tells whether the validation and the translation to
# the canonical format whether worked or failed

RUN	echo "Installing compareIds script" && \
         scif install /opt/scif_app_recipes/compareIds_v1.0_centos7.scif

ENV PATH /scif/apps/compareids/bin:$PATH

#RUN find /scif/apps -maxdepth 2 -name "bin" | while read in; do echo "export PATH=\$PATH:$in" >> /etc/profile.d/myenv.sh;done
#RUN find /scif/apps -maxdepth 2 -name "bin" | xargs -I % echo "ln -s %/* /usr/local/bin/" | bash

#ENTRYPOINT	["/opt/docker-entrypoint.sh"]
#CMD [compareIds.py]

#RUN find /scif/apps -maxdepth 2 -name "bin" | while read in; do echo "export PATH=\$PATH:$in" >> /etc/bashrc;done
#RUN if [ -z "${LD_LIBRARY_PATH-}" ]; then echo "export LD_LIBRARY_PATH=/usr/local/lib" >> /etc/bashrc;fi
#RUN find /scif/apps -maxdepth 2 -name "lib" | while read in; do echo "export LD_LIBRARY_PATH=\$LD_LIBRARY_PATH:$in" >> /etc/bashrc;done
