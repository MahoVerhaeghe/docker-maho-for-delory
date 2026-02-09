# 4.1 Image de base : `python:3.11-slim`
FROM python:3.11-slim

# .
WORKDIR /app

# 4.1 **Fichier `requirements.txt` :**
COPY requirements.txt .

# 4.1 Optimisation du cache pip
RUN pip install --no-cache-dir -r requirements.txt

# .
COPY . .

# 4.1 app.run(host='0.0.0.0', port=5000)
RUN useradd -m pythonuser
USER pythonuser

# 4.2 Ajoutez une instruction HEALTHCHECK au Dockerfile : Intervalle : 30 secondes, Timeout : 10 secondes, Retries : 3, Endpoint : `/health`
HEALTHCHECK --interval=30s --timeout=10s --start-period=5s --retries=3 \
    CMD curl -f http://localhost:5000/health || exit 1

# 4.1 app.run(host='0.0.0.0', port=5000)
EXPOSE 5000

# 4.1 Utiliser gunicorn en production : `gunicorn --bind 0.0.0.0:5000 app:app`
CMD ["gunicorn", "--bind", "0.0.0.0:5000", "app:app"]