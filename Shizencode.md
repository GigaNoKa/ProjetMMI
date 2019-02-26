# ProjetMMI
//Classe "JEU" créer en 3ème.

import shizen.Tuile;

public class Jeu {

	// Classe "principale" qui permet de lancer le jeu.

	public Jeu() {
		
		Modele Z = new Modele();
		Z.Creatab();
		Tuile[][] tab= Z.getTab();
		int avancement = 0;
		while (avancement<288) {
			Z.afficheTab(Z.getTab());	// Affichage du plateau		
			tab = Z.selectionTuiles(Z.getTab()); //sélection des tuiles
			tab = Z.modifLigne(Z.getTab()); // Modifie la ligne après suppression d'une tuile
			tab = Z.modifColonne(Z.getTab()); // Modifie la colonne après suppression d'une tuile
			avancement = avancement +2;	
		}
		
	}
	
	public static void main(String[]args) {
		Jeu partie = new Jeu();
		
		
	}
}


// Classe "MODELE" créer en première 

import java.util.Scanner;

import shizen.Tuile;

public class Modele {

	// Création d'un tableau tuile contenant 288 tuiles.
  
	Tuile[] tab = new Tuile[288];
  
	//Création d'un tableau a 2 dimensions, qui correspond aux nombres de tuiles en x et en y
  
	Tuile[][] tabfini = new Tuile[12][24];
  
	//Création d'un compteur pour le score
  
	int compteur = 0;
  
	// Méthode pour créer le tableau en fonction de leur position, du nombre qu'elle contient et de leur couleur
  
	public void Creatab() {
		int i = 0;
		for (int j = 0; j < 8; j++) {
			
			for (int z = 1; z < 10; z++) {
				tab[i] = new Tuile(z, 1, false);
				i++;
			}
			for (int z = 1; z < 10; z++) {
				tab[i] = new Tuile(z, 2, false);
				i++;
			}
			for (int z = 1; z < 10; z++) {
				tab[i] = new Tuile(z, 3, false);
				i++;
			}
		for (int z = 1; z < 10; z++) {
				tab[i] = new Tuile(z, 4, false);
			i++;
			}
		
		}
    
		// Définition de la taille du Tableau en fonction du nombres de tuile ( ici 288 )
    
		Tuile[][] tabfini = new Tuile[12][24];
		int x = 0;
		int y = 0;
		while (y < tabfini.length) {
			while (x < tabfini[y].length) {
				while (tabfini[y][x] == null) {
				int aleat = (int) (Math.random() * 288);
					if (tab[aleat] != null) {
					tabfini[y][x] = tab[aleat];
						tab[aleat] = null;
					}
				}
				x++;
			}
			x = 0;
			y++;
		}
		this.tabfini = tabfini;
	}
	public Tuile[][] getTab() {
		return tabfini;
	}
  
	//Mise en forme du jeu console pour un côté plus pratique
  
	public void afficheTab(Tuile[][] plateau) {
		System.out.println(
				"      0    1    2    3    4    5    6    7    8    9    10   11   12   13   14   15   16   17   18   19   20   21   22   23 ");
		for (int j = 0; j < 12; j++) {
			if (j < 10) {
				System.out.print("" + j + "  | ");
			} else {
				System.out.print(j + " | ");
			}
			for (int i = 0; i < 24; i++) {
				if (plateau[j][i] == null) {
					System.out.print(" vide");
				} else {
					System.out.print(plateau[j][i].toString());
				}
			}
			System.out.println(" ");
		}
		System.out.println(
				" ---------------------------------------------------------------------------------------------------------------------------");
	}
  
	// Création  de 2 méthodes qui modifie la ligne et la colonne lorsque le joueur joue 2 cases identiques
  
	public Tuile[][] modifLigne(Tuile[][] plateau) {
		for (int y = 1; y < 12; y++) {
			for (int x = 0; x < 24; x++) {
				if (plateau[y][x] == null) {
					for (int z = y; z != 0; z--) {
						plateau[z][x] = plateau[z - 1][x];
						plateau[z - 1][x] = null;
					}
				}
			}
		}
	return plateau;
	}
	public Tuile[][] modifColonne(Tuile[][] plateau) {
		int testdeuxcolonnes = 0;
		while (testdeuxcolonnes != 2) {
			for (int y = 1; y < 12; y++) {
				for (int x = 0; x < 24; x++) {
					int nbrnull = 0;
					if (plateau[y][x] == null) {

						for (int i = 0; i < 12; i++) {
							if (plateau[i][x] == null) {
								nbrnull = nbrnull + 1;
							}
						}
						
						if (nbrnull == 12) {
							for (y = 0; y < 12; y++) {

								for (int z = x; z < 23; z++) {

									plateau[y][z] = plateau[y][z + 1];
									plateau[y][z + 1] = null;
								}
							}
							break;
						}

					}
				}
			}
			testdeuxcolonnes = testdeuxcolonnes + 1;
	}
		return plateau;
	}
  
  // Création Méthode affichant les indications a suivre pour jouer au jeu 

  
  public Tuile [][] selectionTuiles(Tuile [][] plateau){ 

	Scanner sc = new Scanner(System.in);
  
	
	System.out.println("Entrez la valeur en x de la première tuile : "); 
	int x1 =sc.nextInt(); 
	
	
		System.out.println("Entrez la valeur en y de la première tuile : ");
		int y1 =sc.nextInt();
		System.out.println("Entrez la valeur en x de la deuxième tuile : ");
		int x2 =sc.nextInt();
		System.out.println("Entrez la valeur en y de la deuxième tuile : "); 
		int y2 =sc.nextInt();

// Création de conditions affichant un message selon les choix du joueurs

	if (plateau [y1][x1]== null || plateau [y2][x2]==null){
			System.out.println("vous avez sélectionné une case vide rééssayer !");
			afficheTab(plateau); 
			selectionTuiles(plateau); 
	} 
	else { 
			if ((plateau[y1][x1].toString()).equals(plateau[y2][x2].toString())) {
				if (Math.abs(y1-y2)+Math.abs(x1-x2)<=3) {
					System.out.println("Bien joué");
					compteur ++;
					System.out.println("Votre Score est de :"+ compteur );
					plateau [y1][x1] = null; plateau [y2][x2] = null; return plateau;

				} 
				else { 
					System.out.println("Vos tuiles sont trop éloignées");
				selectionTuiles(plateau); 
					afficheTab(plateau);

				} 
			}

			else { System.out.println("Les tuiles sélectionnées ne sont pas identiques");
			selectionTuiles(plateau); afficheTab(plateau);

			} 
		}
	
	return plateau;

	}
}


 // Classe "TUILE" créer en deuxième

public class Tuile {

	// Création d'une méthode tuile qui permet de renvoyer la couleur et le chiffre que contient une Tuile
  
    boolean marque = false;
    public int couleur;
    public int chiffre;
    public Tuile(int couleur_, int chiffre_,boolean marque_) {
            this.couleur=couleur_;
            this.chiffre=chiffre_;
            this.marque = marque_;
    }
    public  int getColor() {
            return couleur;
    }
    public  int getChiffre() {
            return chiffre ;
    }
    public String toString() {
            return ("["+couleur+"|"+chiffre+"]");
    }
}
