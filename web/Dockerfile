# Dockerfile pour le service web (Go)
FROM golang:1.21-alpine AS builder

WORKDIR /app

# Copier dispatcher.go
COPY dispatcher.go .

# Compiler avec options pour un statique binaire
RUN CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -o dispatcher dispatcher.go

# Stage final - utiliser scratch pour un binaire minimal
FROM scratch

WORKDIR /app

# Copier l'exécutable depuis le builder
COPY --from=builder /app/dispatcher .

# Copier le répertoire static
COPY static ./static

# Exposer le port 80
EXPOSE 80

# Lancer le serveur
CMD ["./dispatcher"]

