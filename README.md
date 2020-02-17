# Apuntes-Bases-de-Datos
# Tipos de Lenguajes:

-**Paradigma imperativo**: Indica cómo se hace algo, no qué queremos; detalla los pasos (algoritmo).
>
-**Programación declarativa**: Indica qué se quiere hacer; declara la intención.
**EJEMPLO:**
>var numbers = [1,2,3,4,5]
>var filtered = []
>for(i=0; i < numbers.length; i++)
	>if(numbers[i] > 3)
		filtered.push(numbers[i])

R(read)E(eval)P(print)L(loop)

# Consultas
## SELECT name
En SQL, las mayúsculas y las minúsculas son consultas distintas.

 - {  
    **SELECT** name     
    **FROM** world     
    **WHERE** name **LIKE** 'Y%';    
  }

 

	 - Like se utiliza para expresiones regulares, lo que hará será buscar  
	   un nombre que empiece por Y. El % significa que va desde 0 a n (puede
	   contener más caracteres a mayores).

 - {  
      **SELECT** name    
      **FROM** world   
      **WHERE** name **LIKE** '%y'     
   }
	 - Buscará los nombres que acaben en "y".
 - {   
    **SELECT** name     
    **FROM** world     
    **WHERE** name **LIKE** '%x%'    
  }
	 - Va a buscar un nombre que tenga letras antes de la x y más letras después de la x.
 - {   
    **SELECT** name    
    **FROM** world    
    **WHERE** name **LIKE** 'c%ia'     
  }
	 - Buscará un nombre que empiece por 'c' y termine por 'ia'

 - {    
    **SELECT** name    
    **FROM** world    
    **WHERE** name **LIKE** '%a%a%a%'   
  }
   - Buscará un nombre que contenga 3 o más 'a'. 

 - {  
    **SELECT** name   
    **FROM** world   
    **WHERE** name **LIKE** '_t%'  
  }
   - Buscará un nombre cuya segunda letra sea una 't'.

>Si por ejemplo quisiéramos que encuentre una cadena que tenga 4 o más letras, después del LIKE, pondríamos: '____%' (cuatro guinoes bajos sin espacios y un porcentaje al final). Además, también funciona con números.

 - {  
    **SELECT** name    
    **FROM** world    
    **WHERE** capital = **CONCAT**(name, 'City')   
  }
   - Lo usamos para concatenar expresiones, en este caso buscaría un nombre que estaría formado por el propio nombre y una extensión que en este caso es 'City'.

 - {   
    **SELECT** capital, name     
    **FROM** world     
    **WHERE** capital **LIKE** **CONCAT**('%',name,'%')    
  }
   - Este mostraría la capital y el nombre cuya capital incluye el nombre del país.

 - {   
    **SELECT** capital, name    
    **FROM** world    
    **WHERE** capital **LIKE CONCAT**(name,'_%')   
  }
   - Mostraría la capital y el nombre donde la capital es una estensión del nombre del país.

 - {   
    **SELECT** name, **REPLACE**(capital,name,'') **AS** extension    
    **FROM** world   
    **WHERE** capital **LIKE CONCAT**(name,'_%')   
  }
   - Mostraría el nombre y la extensión donde la capital es una extensión del nombre del país

 > **Otra forma de hacer un "between" para incluir o excluir números/datos:**

>**Original:** {SELECT name, area FROM world WHERE area BETWEEN 250000 AND 300000};
	>
	>**Sin BETWEEN:** {SELECT name, area FROM world WHERE area >= 250000 AND area <= 300000}
		
> **Original:** {SELECT name, population FROM world WHERE name IN ('Sweden', 'Norway', 'Denmark')};
		>
	>**Sin IN:**
		>{SELECT name, population FROM world WHERE name = 'Sweden' OR name = 'Norway' OR name = 'Denmark'}

## SELECT from World
- {  
**SELECT** name, population/1000000 **AS** 'population'    
**FROM** world    
**WHERE** continent = 'South America'   
};
  - El AS lo usamos para renombrar. Nos mostraría el nombre y la población expresada en millones de los países de América del Sur.

- {    
  **SELECT** name, population, area     
  **FROM** world    
  **WHERE** area > 3000000    
    **XOR** population > 2500000000   
  }
  - El equivalente a esta consulta sin XOR sería:  	
    - {    
      **SELECT** name, population, area    
      **FROM** world    
      **WHERE** (area > 3000000 OR population > 250000000)   
      **AND NOT** (area > 3000000 AND population > 250000000);   
  }	

- {   
  **SELECT** name, **ROUND**(population/1000000, 2), **ROUND** (gdp/000000000, 2)    
  **FROM** world    
  **WHERE** continent = 'South America';   
  }
  - ROUND nos sirve para redondear, primero ponemos lo que queremos que redondee y, a continuación, el número de decimales que queremos. En este caso el redondeo sería a dos decimales en ambos.

- {    
  **SELECT** name, capital     
  **FROM** world    
  **WHERE LENGTH**(name) = **LENGTH**(capital);   
  }
  - La función LENGTH sirve para comparar longitudes entre cadenas.

- {   
  **SELECT** name, capital      
  **FROM** world    
  **WHERE LEFT**(name, 1) = **LEFT**(capital, 1)    
  **AND** name <> capital;   
  }
  - Esta consulta nos mostraría los nombres de los países y sus capitales siempre y cuando la capital y el apís no se llamen igual y que el primer caracter de ambos sea igual. En este caso con LEFT primero le indicamos lo que queremos "isolate" (no me sale la palabra en español xD) y con el número le indicamos el caracter exacto.

- {     
  **SELECT** name    
  **FROM** world    
  **WHERE** name **LIKE** '%a%'    
  **AND** name **LIKE** '%e%'    
  **AND** name **LIKE** '%i%'    
  **AND** name **LIKE** '%o%'    
  **AND** name **LIKE** '%u%'    
  **AND** name **NOT LIKE** '% %';   
  }
  - Le planteé al profesro si serviría lo siguiente: name LIKE '%a%e%i%o%u%'; pero no mostraría el mismo resultado que la consulta de arriba ya que mostaría todas en la misma palabra y **seguidas**.

## SELECT within SELECT
- {    
  **SELECT** name    
  **FROM** world    
  **WHERE** population >    
  (**SELECT** population **FROM** world **WHERE** name = 'Russia');   
  }
  - Aquí empleamos una subconsulta en el WHERE para poder comparar todas las poblaciones con la de Rusia. Esto nos mostraría todos los nombres de los países que tengan una población superior a la de Rusia.

- {   
  **SELECT** name, continent    
  **FROM** world    
  **WHERE** continent **IN** ((**SELECT** continent **FROM** world **WHERE** name = 'Argentina'), (**SELECT** continent **FROM** world **WHERE** name = 'Australia'))    
  **ORDER BY** name ASC;   
  }
  - Mostraría el nombre del país y el continente del que contenga a Australia o o a Argentina ordenando ascendentemente por el nomre (orden alfabético).

- {    
  **SELECT** name, **CONCAT**(**ROUND**(100 * population/(**SELECT** population **FROM** world **WHERE** name = 'Germany'),0),'%')     
  **FROM** world    
  **WHERE** continent = 'Europe';   
  }
  - **OTRA FORMA DE HACERLO:**
    - {    
  **SELECT** name    
  **FROM** world    
  **WHERE** population >=    
  **ALL**(**SELECT** population **FROM** world **WHERE** population > 0);   
  }

- {   
  **SELECt** continent, name, area    
  **FROM** world x    
  **WHERE** area >=     
  ALL(**SELECT** area **FROM** world y **WHERE** y.continent = x.continent **AND** population > 0);   
  }
  - Cuando vamos a tomar datos de la misma tabla y consideramos que son distintas les designamos un nombre, como a las variables o a los objetos en Java.
  - En este caso nos mostratía el continente, el nombre del país y el área del más grande, por área, de cada continente.

- {    
  **SELECT** name, continent, population    
  **FROM** world x      
  **WHERE**  25000000 >=    
  **ALL**(**SELECT** population **FROM** world y **WHERE** x.continent = y.continent);   
}
  - Mostraría el país, el continente y la población de los continentes cuyos países tengan una población inferior a  25000000 }

- {    
  **SELECT** name, continent       
  **FROM** world x       
  **WHERE** population > ALL      
  (**SELECT** population*3 **FROM** world y **WHERE** x.continent = y.continent **AND** x.name<> y.name **AND** population IS NOT NULL);     
  }
  - Mostraía el nombre de los países y su continente, de los que tienen una población que es tres veces mayor que sus países vecinos 

## SUM AND COUNT
- {       
  **SELECT** **SUM**(population), **SUM**(gdp)         
  **FROM** bbc           
  **WHERE** region = 'Europe';       
  }
  - Mostraría la población y el gdp total.

- {     
  **SELECT DISTINCT** region          
  **FROM** bbc       
  }
  - Mostraría las regiones.

- {      
  **SELECT** name, population            
  **FROM** bbc          
  **WHERE** population > 100000000        
   **ORDER BY** population **DESC**;         
   }
  - Mostraría el nombre y la pobalción de cada país con una población mayor a 100000000; además, se mostrarán decendentemente por pobalción.

- {        
  **SELECT COUNT**(name)            
  **FROM** world          
  **WHERE** area >= 1000000;           
  }
  - Mostrará el **número** da países que tienen un área mayor o igual que 1000000.

- {        
  **SELECT** continent, **COUNT**(name)          
  **FROM** world         
  **GROUP BY** continent;         
  }
  - Mostrará, por cada continente, el nmbre del continente y el número de países.

- {         
  **SELECT** continent        
   **FROM** world         
   **GROUP BY** continent       
   **HAVING SUM**(population)>= 100000000            
   }
  - Mostrará los continentes que tienen una población total de al menos 100000000.
  - Podríamos decir que HAVING sustituye al WHERE.

- {         
  **SELECT** continent        
  **FROM** world        
  **WHERE** population > 10000000            
  **GROUP BY** continent        
  **HAVING SUM**(population) > 10000000;        
  }
  - Es un ejemplo proporcionado por el profesor. En este caso solo onsideraría las poblaciones de más de 10 millones.

## JOIN
- {       
  **SELECT** matchid, player         
  **FROM** goal        
  **WHERE** teamid = 'GER';            
  }
  - Mostrará el matchid y el nombre del jugador para todos los goles ,arcados por Alemania.

- {     
  **SELECT** player, teamid, stadium, mdate        
  **FROM** game **JOIN** goal **ON**(game.id = goal.matchid)        
  **WHERE** teamid = 'GER';       
  }
  - El FROM une datos de la tabla goal con datos de la tabla game.
  - El ON nos dice qué datos deben coincidir para poder unir las tablas (clave foránea/calve principal), por lo que los id's de ambas tablas deben ser el mismo para poder obtener los datos que queremos.
  - Antes de cada elemento de la tabla, para facilitar su entendimiento, le ponemos el nombre de la tabla de donde lo sacamos.
  - Este es como el anterior pero esta vez tenemos datos de dos tablas y por eso utilizamos la cláusula JOIN. Mostrará el jugador, el teamid, el estadio y la fecha de cada gol marcado por Alemania.

- {     
  **SELECT** team1, team2, player         
  **FROM** game **JOIN** goal **ON**(id = matchid)         
  **WHERE** player **LIKE** 'Mario%'      
  }
  - Mostrará ambos equipos y el nombre del jugador que haya marcado un jugador llamdao Mario

- {       
  **SELECT** mdate, teamname        
  **FROM** game **JOIN** eteam **ON** (team1 = eteam.id)         
  **WHERE** coach **LIKE** 'Fernando Santos'          
  }
  - Mostrará la fecha de los partidos y el nombre del equipo donde 'Fernando Santos' fue el entrenador de team1.
  - En el ON del JOIN también podríamos haber puesto team2 = eteam.id.

- {     
  **SELECT** teamname, **COUNT**(player)    
    **FROM** eteam **JOIN** goal **ON** id = teamid    
    **GROUP BY** teamname      
    }
  
- {       
  **SELECT** game.stadium, **COUNT**(goal.matchid)       
  **FROM** game **JOIN** goal **ON** game.id = goal.matchid         
  **GROUP BY** game.stadium;      
  }
  - Mostrará el estadio y el número de goles marcados en cada uno.

- {         
  **SELECT** goal.matchid, game.mdate, **COUNT**(teamid) **AS** 'Goals'          
  **FROM** game **JOIN** goal **ON** matchid = id          
  **WHERE** teamid = 'GER' **GROUP BY** matchid;       
  }
  - Mostrará, para cada partido de Alemania donde hayan marcado, em matchid, la fecha y el número de goles marcados por ellos. 

- {        
  **SELECT** goal.matchid, game.mdate, **COUNT**(teamid) **AS** 'Goals'           
  **FROM** game **JOIN** goal **ON** goal.matchid = game.id        
  **WHERE** (team1 = 'POL' **OR** team2 = 'POL')         
  **GROUP BY** matchid
  }
  - Mostrará lo mismo que el anterior pero esta vez con Polonia.

- {         
  **SELECT** matchid, mdate, **COUNT**(teamid) **AS** 'Goals'       
  **FROM** game **JOIN** goal **ON** matchid = id     
  **WHERE** teamid = 'GER'         
  **GROUP BY** matchid;      
  }

  > Cuando en un JOIN ponemos LEFT o RIGHT, es para que coja también los valores nulos (siempre y cuando en esa tabla haya atributos con valores nulos).
  > Lo que va en el ON también se puede poner en el WHERE (simplemente hayq ue igualarlos).

  - {        
  **SELECT DISTINCT** d.name               
  **FROM** actor d **JOIN** casting a **ON** (a.actorid = d.id)       
  **JOIN** casting b **ON** (a.movieid = b.movieid)       
  **JOIN** actor c **ON** (b.actorid = c.id **AND** c.name = 'Art Garfunkel')         
  **WHERE** d.id <> c.id;    
  }
    - Mostrará todos los actores y actrices que hayan trabajado con Art Garfunkel.
    - ***IMPORTANTE PARA EL EXAMEN***
