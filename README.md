## Docker Compose


## Introducere
Acest laborator are ca scop aprofundarea utilizării Docker Compose pentru a gestiona o aplicație fullstack formată din trei componente:
- **Frontend** (Angular)
- **Backend** (Node.js - REST API)
- **Database** (PostgreSQL)

În loc să rulăm fiecare componentă manual folosind `docker run`, putem defini toate serviciile într-un fișier `docker-compose.yml` și să le gestionăm mai eficient.

---

## 1. Crearea și rularea containerelor individual
Dacă vrem să rulăm fiecare container separat, trebuie să executăm comenzile de mai jos în ordine.

### **1.1 Crearea imaginii pentru baza de date PostgreSQL**
```sh
docker image build -t laborator/todo-postgres ./database
````

### **1.2 Pornirea containerului pentru baza de date**

```sh
docker container run --name database --network lab4-std -d laborator/todo-postgres
```

### **1.3 Crearea imaginii pentru backend (Node.js)**

```sh
docker image build -t laborator/todo-backend ./backend
```

### **1.4 Pornirea containerului pentru backend**

```sh
docker container run --name todo-backend --network lab4-std -p 3000:3000 -d laborator/todo-backend
```

### **1.5 Crearea imaginii pentru frontend (Angular + Nginx)**

```sh
docker image build -t laborator/todo-frontend ./frontend
```

### **1.6 Pornirea containerului pentru frontend**

```sh
docker container run --name todo-frontend --network lab4-std -p 8080:80 -d laborator/todo-frontend
```

## 2. Testarea manuală a serviciului REST API (backend)

### **2.1 Vizualizarea tuturor task-urilor**

```sh
curl http://localhost:3000/api/tasks
```

### **2.2 Adăugarea unui nou task**

```sh
curl -X POST -d '{"description": "Test"}' -H "Content-Type: application/json" http://localhost:3000/api/tasks
```

### **2.3 Confirmarea adăugării task-ului**

```sh
curl http://localhost:3000/api/tasks
```

---

## 3. Utilizarea Docker Compose

Pentru a simplifica gestionarea aplicației, folosim `docker-compose.yml`. Astfel, putem porni toate containerele cu o singură comandă.

### **3.1 Construirea și rularea întregii aplicații**

```sh
docker-compose up --build
```

### **3.2 Rularea aplicației în background**

```sh
docker-compose up --detach --build
```

### **3.3 Oprirea și ștergerea containerelor**

```sh
docker-compose down
```

## 4. Rularea în modul de dezvoltare

Pentru dezvoltare, putem folosi `docker-compose-dev.yml`, care permite montarea codului sursă din directorul local pentru un feedback rapid.

### **4.1 Construirea și rularea aplicației în mod dezvoltare**

```sh
docker-compose -f docker-compose.yml -f docker-compose-dev.yml up -d --build
```

---

## 5. Diferența între rularea manuală și utilizarea Docker Compose

| Metodă                                   | Avantaje                                                                                          | Dezavantaje                                    |
| ---------------------------------------- | ------------------------------------------------------------------------------------------------- | ---------------------------------------------- |
| **Rulare manuală (`docker run`)**        | Control mai detaliat, personalizare pe fiecare container                                          | Necesită multiple comenzi, risc de erori umane |
| **Docker Compose (`docker-compose up`)** | Ușor de gestionat, pornește toate containerele cu o singură comandă, include dependențe și rețele | Necesită un fișier YAML bine configurat        |

---

## 6. Concluzie

Docker Compose simplifică semnificativ gestionarea aplicațiilor multi-container, reducând riscul de erori și facilitând scalarea și mentenanța. Acest laborator a demonstrat cum să construim și să rulăm o aplicație fullstack eficient folosind Docker și Docker Compose.
