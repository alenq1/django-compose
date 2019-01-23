FROM python:3.6
ENV PYTHONUNBUFFERED 1
ENV PYTHONDONTWRITEBYTECODE 1
RUN mkdir -p /webapp
#    && mkdir -p /webapp/static \
#    && mkdir -p /webapp/media
#ADD ../config/ /config
WORKDIR /webapp
ADD . /webapp
RUN pip install -r /webapp/requirements.txt
RUN python manage.py collectstatic --no-input
EXPOSE 8000
CMD ["gunicorn", "--bind", ":8000", "webapp.wsgi:application"]