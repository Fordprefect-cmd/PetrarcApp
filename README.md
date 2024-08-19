# Descrizione Progetto 

È un editor di testi musicali e poetici, analizza un testo fornito attraverso le regole della metrica italiana e ne mostra le proprietà ritmiche.
La metrica è l’insieme di regole e concetti che regolano e misurano la ritmica e musicalità di un testo.
Al momento il programma consiste nel codice python che attraverso una rudimentale interfaccia in React prende un testo, e genera 2 dataframe che ne riassumono le proprietà, uno per ciascuna parola e uno per ogni riga di testo rispettivamente. 
Le regole della metrica necessarie per descrivere un testo sono programmate nella funzione process_string.     La quale fa uso di 3 concetti fondamentali per eseguire tutte le operazioni di analisi metriche (https://github.com/Fordprefect-cmd/petrarc-app-react/blob/2c2d0ecc0038c0f4e97edbd6fd2294aab864f463/La%20metrica%20italiana%20(Pietro%20G.%20Beltrami)%20(Z-Library)-pages-2.pdf).
1)	L’unita minima di base in cui dividere il testo da analizzare è la SILLABA, che si ottiene spezzando ciascuna parola (notare a proposito questi articoli https://accademiadellacrusca.it/it/consulenza/divisione-in-sillabe/302).
2)	Quali sillabe in ogni parola contengono o meno una vocale accentata (detta tonica).
3)	Quanto è lungo il verso (una riga di testo) in sillabe, e come calcolare questo numero.

  ### Per realizzare quanto detto al punto 3), è stato realizzato un database di parole italiane con sillabazione e accento, il primo di questo tipo per la lingua italiana pubblicamente disponibile, salvato sotto "df_cleaned (1).csv" nella cartella "src". 
  
Il programma si occupa di ricavare e tenere traccia di questi elementi per descrivere accuratamente la ritmica e musicalità del testo attraverso un’interfaccia simile a quella delle IDE. 
Segue un breve schema dell’ordine cronologico delle operazioni che il programma python svolge sul testo, per una spiegazione dettagliata del codice fare riferimento al file Flask_app.py nella repository, questo file è una copia offline di quello a cui fa riferimeno l’app flask per la server request, contiene tutte le funzioni citate di seguito.
Schema Ordine di popolamento delle colonne – Tabella df_Principale

1)	Parola (str): splitta il multiline user imput agli spazi e newlines, contiene tutte le parole nel testo creato dall’utente 

2)	Trovata (bool): controlla se ogni parola nella colonna “Parola (str)” viene trovata nel Dizionario oppure no.

3)	Sillabazione(str): Se la parola viene trovata nel dizionario, ne viene estratta la relativa Sillabazione (stringa divisa da “-”). Se non viene trovata, la stringa di sillabazione è generata dalla funzione SillaBot().


4)	 NumElementiSillabazione(Int): conta il numero sillabe di ogni stringa della colonna Sillabazione
 
5)	PosizioneAccento(Int): Individua l’indice della vocale accentata di ogni stringa della colonna Sillabazione


6)	Substringa_Rima(str): Viene ottenuta estraendo la parte della sillabazione che segue la vocale accentata.

7)	Ultima_Parola(Bool): Viene determinato se la parola è l'ultima nella riga.


8)	DiexSin_eresi(str/bool): Basato su colonna Substringa_Rima(str), si usa la funzione check_sineresi_dieresi() per determinare se la parola rispecchia le condizioni per sineresi o dieresi (assegnando le relative stringhe) oppure no (valore: false). 

9)	 Sinalefe (Bool): Viene calcolato utilizzando la funzione Sinalefe() sulla base dello User imput come per la colonna “Parola”, per verificare la presenza di sinalefe nella riga.

10)	Range_vocali(lista di int): Sulla base della colonna “Parola”, viene calcolato utilizzando la funzione trova_range_vocali() per trovare i range degli indici delle vocali adiacenti in una parola. Utile per evidenziare la parte rilevante di una parola per le condizioni sopracitate.


11)	 TipoDiParola(str): Sulla base delle colonne “Sillabazione(str)” e “PosizioneAccento(Int)”, viene calcolato utilizzando la funzione tipo_di_parola() per catalogare le parole in base alla posizione dell'accento dalla fine della parola. 

12)	Alternative(lista di stringhe): Sulla base delle colonne “Sillabazione(str)” e “NumElementiSillabazione (Int)” Viene ottenuta utilizzando la funzione cerca_parole_alternative() per trovare parole alternative che fanno rima e con lo stesso numero di sillabe.

13)	Riga_testo_indice_parola(str): Aggiunta per ultima al di fuori di process_string(), assegna ad ogni entry un codice che rappresenta la posizione nel testo. Espresso come [“Indice riga di testo”.”indice parola nella riga”]. Per esempio una parola che ha un valore di 3.5 significa che è la quinta parola sulla terza riga. Questo è utile per tenere traccia in modo univoco di parole che potrebbero avere lo stesso valore nella colonna “parola” ma diversi valori nella colonna Sillabazione o Posizione accenti per scelta degli utenti.
 
Schema Ordine di popolamento delle colonne – Tabella df_Riassuntivo
Una riga di questa tabella (df_Riassuntivo) corrisponde alle informazioni su una sola riga di poesia, detta verso. Mentre per la tabella df_Principale una riga corrisponde alle informazioni su una sola Parola, le quali sono mappate alla loro riga di testo grazie alla colonna Riga_testo_indice_parola.

1)	conto_assoluto: Questa colonna rappresenta il totale delle sillabe di tutte le parole nel verso (riga di testo).
2)	Totale_Sineresi: Questa colonna rappresenta il conteggio totale di sineresi 
3)	Totale_Sinalefe: Questa colonna rappresenta il conteggio totale di sinalefe 
4)	Totale_Dieresi: Questa colonna rappresenta il conteggio totale di dieresi.
5)	posiz_acc_in_verso: Questa colonna rappresenta la lista degli indici delle sillabe con vocali accentate relative al verso. Basata sulle colonne “Sillabazione”, “DiexSin_eresi” e “Sinalefe”. 
6)	computo_finale: Questa colonna rappresenta il calcolo metrico delle sillabe del verso, che è il risultato del conto_assoluto meno il totale delle colonne Totale_Sineresi, Totale_Sinalefe e Totale_Dieresi.












# Getting Started with Create React App

This project was bootstrapped with [Create React App](https://github.com/facebook/create-react-app).

## Available Scripts

In the project directory, you can run:

### `npm start`

Runs the app in the development mode.\
Open [http://localhost:3000](http://localhost:3000) to view it in the browser.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm test`

Launches the test runner in the interactive watch mode.\
See the section about [running tests](https://facebook.github.io/create-react-app/docs/running-tests) for more information.

### `npm run build`

Builds the app for production to the `build` folder.\
It correctly bundles React in production mode and optimizes the build for the best performance.

The build is minified and the filenames include the hashes.\
Your app is ready to be deployed!

See the section about [deployment](https://facebook.github.io/create-react-app/docs/deployment) for more information.

### `npm run eject`

**Note: this is a one-way operation. Once you `eject`, you can’t go back!**

If you aren’t satisfied with the build tool and configuration choices, you can `eject` at any time. This command will remove the single build dependency from your project.

Instead, it will copy all the configuration files and the transitive dependencies (webpack, Babel, ESLint, etc) right into your project so you have full control over them. All of the commands except `eject` will still work, but they will point to the copied scripts so you can tweak them. At this point you’re on your own.

You don’t have to ever use `eject`. The curated feature set is suitable for small and middle deployments, and you shouldn’t feel obligated to use this feature. However we understand that this tool wouldn’t be useful if you couldn’t customize it when you are ready for it.

## Learn More

You can learn more in the [Create React App documentation](https://facebook.github.io/create-react-app/docs/getting-started).

To learn React, check out the [React documentation](https://reactjs.org/).
