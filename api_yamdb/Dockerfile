FROM python:3.7-slim
WORKDIR /app
COPY ./ .
RUN pip install -r requirements.txt
WORKDIR /app/api_yamdb/
RUN python manage.py collectstatic --noinput
CMD ["gunicorn", "api_yamdb.wsgi:application", "--bind", "0:8000" ]