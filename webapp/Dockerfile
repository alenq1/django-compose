# Set the image version and source os
ARG PYTHON_VBASE=3.7.2-slim-stretch
ARG PYTHON_VFINAL=3.7.2-slim-stretch
#Download the image to create a initial builder
FROM python:$PYTHON_VBASE as builder

#Install the python libriaries into requirements folder
ARG SCENARIO=base
COPY requirements/$SCENARIO.txt /
#create wheels directory for compiled libraries
RUN pip install --upgrade pip \
    && pip wheel --no-cache-dir --no-deps --wheel-dir /wheels -r $SCENARIO.txt
##    && pip install --install-option="--prefix=/install" -r /$SCENARIO.txt


#ADD ../config/ /config


## If you want to create additional folders
#    && mkdir -p /webapp/static \
#    && mkdir -p /webapp/media
# 
#######Final Base Image
FROM python:$PYTHON_VFINAL as base
ARG SCENARIO=base
#Enviroment to python image
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1
#Install curl for healthcheck in this image
RUN apt-get update \  
    && apt-get install --no-install-recommends --no-install-suggests -y curl \
    && rm -rf /var/lib/apt/lists/*    
#Copy wheels directory from builder image to final image
COPY --from=builder /wheels /wheels
COPY --from=builder $SCENARIO.txt .
RUN pip install --no-cache /wheels/*

#Create a new user for execute the container instead root for security purposes
RUN groupadd app && useradd -g app app
RUN mkdir -p /webapp  /webapp/media /webapp/static \
&& chown -R app:app /webapp 
WORKDIR /webapp
ADD . /webapp

ENV PORT 8000
EXPOSE $PORT
USER app
#ENTRYPOINT [ "entrypoint.sh" ]

###############################################
FROM python:$PYTHON_VFINAL as local
ARG SCENARIO=local
#Enviroment to python image
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1
#Install curl for healthcheck in this image
RUN apt-get update \  
    && apt-get install --no-install-recommends --no-install-suggests -y curl \
    && rm -rf /var/lib/apt/lists/*    
#Copy wheels directory from builder image to final image
COPY --from=builder /wheels /wheels
COPY --from=builder $SCENARIO.txt .
RUN pip install --no-cache /wheels/*

#Create a new user for execute the container instead root for security purposes
RUN groupadd app && useradd -g app app
RUN mkdir -p /webapp  /webapp/media /webapp/static \
&& chown -R app:app /webapp 
WORKDIR /webapp
ADD . /webapp

ENV PORT 8000
EXPOSE $PORT
USER app
#ENTRYPOINT [ "entrypoint.sh" ]