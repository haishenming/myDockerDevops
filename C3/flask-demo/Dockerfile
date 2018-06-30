FROM python:3.6.5
LABEL maintainer="haishenmingx<haishenmingx@gmail.com>"
RUN pip install flask
COPY app.py /app/app.py
WORKDIR /app
EXPOSE 5000
CMD [ "python", "app.py" ]
