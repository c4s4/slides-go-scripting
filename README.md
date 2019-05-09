# Go Scripting

Michel Casabianca

casa@sweetohm.net

http://sweetohm.net

---
## Comment scripter en Go ?

Considérons le source Go suivant :

```go
package main

func main() {
    println("Hello World!")
}
```

Nous pouvons l'exécuter avec la commande :

```bash
$ go run hello.go 
Hello World!
```

Pour ce faire, il faut que Go ait été installé bien sûr, et que la commande *go* soit dans le *PATH*.

À noter que d'autres sources du répertoire ne seront pas importés. Le script peut donc utiliser les bibliothèques standards du Go ou d'autres installées avec `go get`, mais le script ne peut comporter qu'un seul source.

---
## Performances

En termes de performance, c'est tout à faire convenable :

```bash
$ time go run hello.go 
Hello World!

real	0m0,117s
user	0m0,120s
sys	0m0,024s
```

À titre de comparaison, un script identique en Python :

```bash
$ time python hello.py 
Hello World!

real	0m0,034s
user	0m0,028s
sys	0m0,004s
```

C'est plus rapide, mais le temps de lancement du script Go reste raisonnable.

---
## Sous le capot

Go commence par compiler le source et place le binaire dans un répertoire de */tmp* :

```bash
$ tree /tmp/go-build527477417/
/tmp/go-build527477417/
└── b001
    ├── exe
    │   └── hello
    ├── importcfg
    ├── importcfg.link
    └── _pkg_.a

2 directories, 4 files
```

Ce qui veut dire que le Go compilé notre source en une centaine de millisecondes !

Rappelons que le binaire généré est lié statiquement, ce qui veut dire que l'on peut le copier sur n'importe quelle machine, pour laquelle on peut compiler un source Go, et exécuter ce binaire sans rien installer d'autre, pas de VM ni de bibliothèque.

---
## Vitesse de compilation

Comparons les temps de compilation entre Go, Java et GCC :

```bash
$ time go build hello.go

real	0m0,047s
user	0m0,028s
sys	0m0,020s
```

```bash
$ time javac HelloWorld.java 

real	0m0,696s
user	0m1,488s
sys	0m0,060s
```

```bash
$ time gcc hello.c 

real    0m0,058s
user    0m0,036s
sys     0m0,012s
```

---
## Cerise sur le gâteau

Pour faire de notre source Go un véritable script, que l'on peut appeler en ligne de commande avec `./hello.go`, il nous faut le rendre exécutable :

```bash
$ chmod +x hello.go
```

Mais cela ne suffit pas car Linux ne sait pas avec quoi exécuter ce script. Il nous faut ajouter une ligne au début du source, que l'on appelle un *shebang* :

```go
//usr/bin/env go run $0 "$@" ; exit

package main

func main() {
    println("Hello World!")
}
```

Nous pouvons maintenant invoquer notre script comme n'importe quel autre :

```bash
$ ./hello.go 
Hello World!
```

---
## Pourquoi scripter en Go ?

Nous avons vu qu'en termes de performance et d'usage, les scripts en Go sont une solution viable.

Mais quel peut être l'intérêt de déployer des scripts en Go sur une machine si on peut le faire en Python par exemple ?

Dans les deux cas il faut installer le moteur sur les machines où l'on déploie les scripts :

- VM dans le cas de Python
- Compilateur pour Go

Mais dans le cas de Python, il faut créer un *virtualenv* pour chaque script ou application que l'on déploie. On y installe toutes les dépendances utilisées par l'application. À l'usage, cela s'avère tellement compliqué à mettre en œuvre que l'on préfère souvent livrer les applications Python dans des conteneurs comme Docker.

---
## Exemple de mise en œuvre

J'ai dû faire un script pour envoyer un message sur Slack après passage de tests d'intégrations sur notre machine de dev. Cela consiste en un appel REST sur l'API de Slack. Pour ce faire avec Python, il me faudrait (hormis l'installation de la VM Python) :

- Créer un *virtualenv* pour le script
- Y installer *requests* pour réaliser facilement les appels HTTP
- Déployer le script

Avec Go, il n'y a qu'à déployer le script. Le script en question est dans le slide suivant :

---
Le script, sans shebang, imports et constantes est le suivant :

```go
func main() {
	if len(os.Args) != 2 {
		fmt.Println("You must pass message on command line")
	}
	message, err := json.Marshal(map[string]string{"text": os.Args[1]})
	if err != nil {
		panic(err)
	}
	request, err := http.NewRequest("POST", urlDev, bytes.NewBuffer(message))
	if err != nil {
		panic(err)
	}
	request.Header.Set("Content-Type", "application/json")
	client := &http.Client{}
	response, err := client.Do(request)
	if err != nil {
		panic(err)
	}
	defer response.Body.Close()
	if response.StatusCode != 200 {
		fmt.Println("Bad status code:", string(response.Status))
		body, _ := ioutil.ReadAll(response.Body)
		fmt.Println("Response body:", string(body))
		os.Exit(1)
	}
}
```

---
## Cheval de Troie

Les scripts en Go sont un bon moyen de faire entrer ce langage dans votre entreprise, en douceur, mais **il ne faut pas le dire à votre boss :o)**.

![](img/cheval-de-troie.png)

---
## Merci pour votre attention

Des questions ?
