# Workshop Android- CookieClicker
**Prérequis**
* Connaître les bases de java.
* Avoir installé Android Studio
* Avoir un smartphone Android ou un simulateur.



## Comment fonctionne android ?
La première chose à comprendre avec android studio, c'est la notion d'activité.
Toutes les applications d'android sont basé sur le schéma suivant. 

![Schéma des activités d'android](./activity.jpg)

Si on veut vulgariser au plus simple, à chaque fois que tu changes de "page" sur ton appli android, tu changes en fait d'activité.

Une ``Activity`` est donc une class qui un schéma de fonctionnement très précis. Il faut comprendre et respecter le mécanisme de fonctionnement d'android. 

On doit utliser toutes les méthodes qui sont indiqués. Certaines sont obligatoires comme la méthode ``onCreate();```

## C'est parti !
Allez dans le dossier ``AndroidStudioProject`` 
Pour linux, il se trouve normalement dans 
````
/home/user/AndroidStudioProject
````

Pour windows : 
````
C:\Users\$YOURDESKTOP\AndroidStudioProjects
````

Ensuite créer un fichier 
````
mkdir WorkshopAndroid
````
Ensuite 
````
git clone https://github.com/LudovicPatho/Workshop-Android-CookieClicker.git
````

Ensuite ouvrer le projet dans android studio.

### Création des layouts.

Allez dans le fichier ``app\res\layout``

Effacer entièrement le fichier, on va partir de 0

#### Création du layout splash screen

Afin d'acceuillir le joueur, nous allons créer un splash screen. Les interface d'android utilisent le xml pour ce qui est du visuel. 

Andoird dispose de plein de layouts différents, je vous invite à regarder dans la documentation. 

Ici on va se servir d'un ``LinearLayout``. En gros, à chaque qu'on insert une composant, il se mets à la ligne suivante. 

````xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:theme="@android:style/Theme.Holo.NoActionBar.Fullscreen"
    android:orientation="vertical"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:layout_gravity="center"
    android:background="@color/colorWhitddde">
````


Il va souligner "@color/colorWhite". En effet il faut aller créer la couleur.
Ouvrez le fichier ``app/res/values/colors.xml``

Et ajoutez le code suivant dans `<resources></resources>`: 
````xml 
<color name="colorWhite">#fff</color>
````

Très bien retournez sur le fichier activity_main.xml 
On va mettre un petit logo 

````xml
<ImageView
        android:id="@+id/logo"
        android:layout_width="150dp"
        android:layout_height="150dp"
        android:src="@drawable/cookie"
        android:layout_gravity="center"
        android:layout_marginTop="150dp"/>
````

Et on va ajouter notre titre 
````xml
 <TextView
        android:id="@+id/nameApplication"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/app_name"
        android:fontFamily="sans-serif"
        android:textStyle="bold"
        android:textSize="40dp"
        android:layout_gravity="center"
        android:textColor="@color/colorAccent" />
````

On ajoute un petit slogan 


````xml
<TextView
        android:id="@+id/slogan"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Cookie Clicker Game"
        android:fontFamily="sans-serif"
        android:textStyle="bold"
        android:textSize="14dp"
        android:layout_gravity="center"
        android:textColor="@color/colorAccent" />

````

Et ensuite un boutton qui permettra au joueur de commencer le jeu.

````xml
    <Button
        android:id="@+id/btnStart"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="C'est parti !"/>
````

Retenez bien les id's, on devoir s'en servir.


#### Le code dans MainActivity

Allez dans le MainActivity.

On va dans un premier déclarer les variables qui sont relatives au layout. Dans ce cas-ci on doit juste récupérer le bouton, puisque il n'y a que lui qui nécéssite une action. 

Déclarez la variable suivante en **global**. 

````java

 private  Button btnStart;
 ````

 Ensuite on va l'assigner dans méthode ``onCreate()``
 ````java 
 protected void onCreate(Bundle savedInstanceState) {

        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        this.btnStart = findViewById(R.id.btnStart);
}
````

Ensuite on va lui mettre un Listener pour l'action boutton. Intent permet d'effectuer une action et en l'occurence il s'agit de changer d'activiter. 

````java
 btnStart.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        Intent otherActivity = new Intent(getApplicationContext(),GameActivity.class);
    }
});
````

Nous devns démarrer la future activity et finir l'activité courante. On rajoute ceci après l'intent
````java
    startActivity(otherActivity); // On démarre l'autre activité
    finish(); // On finit l'activité courante.
````

Très bien, aller dans le fichier manifest.xml et entre les balises application rajoutez ceci :
````xml
  <activity android:name=".GameActivity" />
````


**Layout de game_activity**

On va tout d'abord défibir un background
````xml
<?xml version="1.0" encoding="utf-8"?>
<android.support.constraint.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".GameActivity"
    android:background="@drawable/background"
    >
````

Cette fois nous allons utiliser le Relative Layout 
````xml
<RelativeLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent">
</RelativeLayout>
````

Entre ces deux balises vous rajoutez ceci, il s'agit du cookie 

````xml 
  <ImageView
            android:id="@+id/cookie"
            android:layout_width="250dp"
            android:layout_height="250dp"
            android:src="@drawable/cookie"
            android:layout_centerHorizontal="true"/>
````

Ensuite on va rajouter un textview pour les points

````xml
   <TextView
            android:id="@+id/points"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Points"
            android:layout_centerHorizontal="true"
            android:layout_centerVertical="true"
            android:textColor="@color/colorWhite"
            />
````

#### Le code dans Game Activity.
Dans ce context ci, nous avons besoin de deux variables pour les views.
Une pour afficher les points, et une autre ppour récupérer l'image. On doit les déclarer en global.


````java 
    private TextView pointsView;
    private ImageView cookie;
````

On doit également créer une variable qui comptera le nombre de clicks effectuer par le joueur


````java
int clicks = 0;
````

On assigne les variables dans la méthode onCreate()

````java
    this.pointsView = findViewById(R.id.points);
    this.cookie = findViewById(R.id.cookie);
````

On met un Listener sur le cookie 

````java
     cookie.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View v) {
                
            }
        });
````

Et dans la méthode onClick, on incrémente la variable clicks et on l'affiche
 ````java

    points++;
    pointsView.setText("Points : " + points);

 ````

 Et on run l'application pour voir ce que cela donne. 





















































