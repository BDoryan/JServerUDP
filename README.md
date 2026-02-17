# JServerUDP

Petit framework Java pour client/serveur UDP base sur des paquets JSON (Gson). Le repo fournit un serveur, un client, un systeme de packets extensible, et un exemple Ping/Pong avec attente de reponse.

## Fonctionnalites

- Communication UDP simple avec serialisation JSON (Gson)
- Systeme de packets typés avec gestion SEND/REPONSE
- Attente de reponse cote client (ex: ping)
- Logs cote client et serveur

## Prerequis

- Java 8+ (ou compatible)
- Dependances en local dans [libs/](libs/)
  - gson-2.6.2.jar (utilise)
  - autres jars presents mais non requis pour l'exemple

## Structure

- [src/jserverudp/StartServer.java](src/jserverudp/StartServer.java) : point d'entree serveur
- [src/jserverudp/StartClient.java](src/jserverudp/StartClient.java) : point d'entree client
- [src/jserverudp/server/Server.java](src/jserverudp/server/Server.java) : loop UDP serveur
- [src/jserverudp/client/Client.java](src/jserverudp/client/Client.java) : loop UDP client + ping
- [src/jserverudp/packets/](src/jserverudp/packets/) : packets, reponses, ping/pong

## Demarrage rapide

Compiler tout le projet:

```bash
mkdir -p bin
javac -cp "libs/*" -d bin $(find src -name "*.java")
```

Lancer le serveur:

```bash
java -cp "libs/*:bin" jserverudp.StartServer
```

Lancer le client:

```bash
java -cp "libs/*:bin" jserverudp.StartClient
```

## Utilisation

- Le client envoie un `PingPacket` et attend un `PongPacket` via `getPing()`.
- Le serveur repond automatiquement au ping dans `PingPacket.handleFromServer()`.
- Pour ajouter un nouveau packet:
  1. Creer une classe qui etend `Packet`.
  2. (Optionnel) Creer une `PacketReponse` associee.
  3. Implementer `handleFromServer` et/ou `handleFromClient`.

## Notes

- UDP ne garantit pas la livraison ni l'ordre des messages.
- Les logs sont ecrits sur la sortie standard (et dans `logger/` cote serveur).
