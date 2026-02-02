# Rezolvarea Numerică a Ecuației de Difuzie a Căldurii (2D)

Acest proiect reprezintă implementarea în Python a metodei diferențelor finite pentru rezolvarea ecuației cu derivate parțiale a difuziei căldurii pe un domeniu dreptunghiular.

## Descrierea Proiectului

Scopul proiectului este modelarea numerică a difuziei căldurii pe un domeniu dreptunghiular $\Omega = [0,2] \times [0,1]$

### Modelul Matematic
Ecuația principală rezolvată este:
$$-\nabla[k(x,y)\nabla u(x,y)]=f(x,y)$$
unde $k(x,y)$ reprezintă conductivitatea termică, iar $f(x,y)$ este sursa de căldură.

### Condiții la Frontieră
Proiectul tratează un caz mixt cu:
1.  **Date Dirichlet ($\Gamma_D$):** Aplicate pe laturile $y=0$, $y=1$ și $x=2$, unde temperatura $u(x,y)$ are valori fixe ($v_1, v_2, v_3$).
2.  **Date Neumann ($\Gamma_N$):** Aplicate pe latura $x=0$, specificând fluxul de căldură $s$ prin relația $-k(x,y)\frac{\partial u}{\partial x} = s$.

### Conductivitatea Materialului
Modelul ia în considerare un mediu neomogen, conductivitatea $k(x,y)$ fiind definită pe 4 zone distincte de interes ($t_1, t_2, t_3, t_4$).

#### Important!
În elaborarea proiectului a fost omisă verificarea și impunerea condiției de continuitate la frontieră a funcției $k(x,y)$ care ia valori diferite pe cele 4 zone distincte de interes ($t_1, t_2, t_3, t_4$) ale dreptunghiului. Din acest motiv modelul **NU** funcționează precis pentru orice condiții anterioare introduse. Totuși, încă reflectă realist modelarea căldurii pe un dreptunghi în două dimensiuni.

## Implementare

Soluția este implementată folosind **JupyterLab** și librăria **NumPy**.

### Metodologie
1.  **Discretizarea Domeniului:**
    * Se utilizează o rețea de $(n+1) \times (m+1)$ noduri.
    * Pasul de discretizare $h$ este uniform, ales astfel încât $n+1 = 2(m+1)$, rezultând $h_1 = h_2 = h$.

2.  **Aproximarea Derivatelor:**
    * Pentru nodurile interioare se folosește metoda diferențelor finite centrale de ordin 2.
    * Pentru condiția Neumann ($x=0$) se folosește interpolarea Lagrange de ordin 2.

3.  **Sistemul Liniar:**
    * Problema se reduce la rezolvarea sistemului $A \cdot U = F$.
    * Rezolvarea efectivă se face prin factorizare LU și metoda Gauss cu pivotare parțială (`numpy.linalg.solve`).

## Rezultate

Proiectul include vizualizarea rezultatelor și validarea metodei:
* **Heatmap:** Reprezentarea grafică a soluției $U$ folosind interpolare biliniară (spline liniar).
* **Analiza Erorii:** Compararea soluției numerice cu o soluție exactă pentru un caz particular. Graficul logaritmic demonstrează că eroarea maximă scade odată cu pasul $h$, având o pantă de ordinul $O(h^2)$.

## Utilizare

Pentru a rula proiectul:
1.  Asigurați-vă că aveți instalată distribuția **Anaconda** sau un mediu Python cu `numpy` și `matplotlib`.
2.  Deschideți fișierul în **JupyterLab**.
3.  Rulați celulele secvențial pentru a genera matricea sistemului și graficele rezultate.
