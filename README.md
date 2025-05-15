# -DATAPROJECT-PAU-CONSULTAS-DE-SQL
Proyecto Módulo SQL de Paulina Galarza
En esta parte del Read Me incluyo el ejecicio "DataProject" por si no es legible el SQL
select *
from film f ;

-- 2. Muestra los nombres de todas las películas con una clasificación por edades de ‘Rʼ.
select "title" as "Nombre Película",
	"rating" 
from film f 
where "rating" = 'R';

-- 3. Encuentra los nombres de los actores que tengan un “actor_idˮ entre 30 y 40
select concat ("first_name", ' ', "last_name") as "Nombre Actores"
from "actor"
where actor_id between 30 and 40;

--4. Obtén las películas cuyo idioma coincide con el idioma original.
select *
from "language";

select "title" as "Nombre Película"
from film f
inner join language l ON f.language_id = l.language_id
where f.original_language_id is not null
  and f.language_id = f.original_language_id;

--5. Ordena las películas por duración de forma ascendente.
select "title" as "Nombre Películas",
	length as "Duración"
from film f 
order by "length" ASC;

--6.  Encuentra el nombre y apellido de los actores que tengan ‘Allenʼ en su apellido.
select "first_name",  "last_name"
from "actor"
where "last_name" like '%ALLEN%';

--7. Encuentra la cantidad total de películas en cada clasificación de la tabla “filmˮ 
--y muestra la clasificación junto con el recuento.
select "rating",
		count("title") as "Total Películas"
from film f
group by "rating";

--8. Encuentra el título de todas las películas que son ‘PG13ʼ o tienen una duración mayor a 3 horas en la tabla film.
select "title" as "Películas",
	"rating",
	"length"  as "Duración"
from film f
where "rating" = 'PG-13' or length > 180;

--9. Encuentra la variabilidad de lo que costaría reemplazar las películas.
select round(VARIANCE ("replacement_cost"), 2) as "Varianza de Reeemplazar Películas"
from film f;

--10. Encuentra la mayor y menor duración de una película de nuestra BBDD.
select MAX("length") as "Duración Mayor",
	MIN("length") as "Duración Menor"
from film f;

--11. Encuentra lo que costó el antepenúltimo alquiler ordenado por día.
select "rental_id" as "Antepnúltimo Alquiler", --antepenultimo alquiler
	rental_date 
from rental r 
order by "rental_date" desc --11,676 2006-02-14
OFFSET 2 LIMIT 1;

select *
from payment p
where p.rental_id = 11676;

select r."rental_id" as "Antepnúltimo Alquiler",
		r. rental_date as "Fecha",
		SUM (p.amount)
from rental r
inner join payment p on r."rental_id" = p."rental_id"
where r.rental_id = (
	select "rental_id" as "Antepnúltimo Alquiler" 
	from rental r 
	order by "rental_date" DESC
	offset 2 limit 1)
group by r."rental_id" ;

--12. Encuentra el título de las películas en la tabla “filmˮ que no sean ni ‘NC- 17ʼ ni ‘Gʼ en cuanto a su clasificación
select "title" as "Películas",
		"rating"
from film f
where rating <> 'NC-17' and rating <> 'G';

--13. Encuentra el promedio de duración de las películas para cada clasificación de 
--la tabla film y muestra la clasificación junto con el promedio de duración
select "rating",
round (AVG("length"), 2) as "Duración"
from film f 
group by "rating";

--14.Encuentra el título de todas las películas que tengan una duración mayor a 180 minutos.
select "title" as "Película",
	"length" as "Duración"
from "film"
where length > 180;

--15. ¿Cuánto dinero ha generado en total la empresa?
select SUM(amount) as "Total Generado"
from payment p;

--16. Muestra los 10 clientes con mayor valor de id.
select "customer_id" as "Clientes",
concat("first_name", ' ', "last_name") as "Nombre Clientes"
from "customer" c
order by "Clientes" desc
limit 10;

--17.Encuentra el nombre y apellido de los actores que aparecen en la película con título ‘Egg Igbyʼ.
select *
from film_actor fa ;

select concat(a."first_name", ' ', a."last_name") as "Nombre Actores",   --artista
		f."title" as "Película"											--film
from "film"  f
inner join "film_actor" fa   										--film actor 
on f."film_id" = fa.film_id
inner join "actor" a
on a."actor_id" =fa."actor_id"
where f.title = 'EGG IGBY';

--18. Selecciona todos los nombres de las películas únicos.
select distinct("title") as "Películas Únicas"
from film f ;

--19.  Encuentra el título de las películas que son comedias y tienen una duración mayor a 180 minutos en la tabla “filmˮ.
select *
from "category" c; --category id pero quiero name

select f."title" as "Películas",
		c."name" as "Categoría",
		f. length as "Duración"
from "film" f
inner join "film_category" fc
on f."film_id" = fc.film_id
inner join "category" c
on c.category_id = fc.category_id
where f.length > 180 and c."name" = 'Comedy';

--20.  Encuentra las categorías de películas que tienen un promedio de duración superior a 110 minutos 
--y muestra el nombre de la categoría junto con el promedio de duración.
select round(avg(f."length"),2) as "Promedio de Duración",
		c."name" as "Categoría"
from "film" f
inner join "film_category" fc
on f."film_id" = fc.film_id
inner join "category" c
on c.category_id = fc.category_id
group by "name" 
having avg(f."length") >110;

--21. ¿Cuál es la media de duración del alquiler de las películas?
select AVG(return_date-rental_date) as "Duración Media Alquiler"
from rental r ;

--22. Crea una columna con el nombre y apellidos de todos los actores y actrices.
select concat("first_name", ' ', "last_name") as "Lista Actores"
from "actor";

--23. Números de alquiler por día, ordenados por cantidad de alquiler de forma descendente.
select count("rental_date") as "Número por día",  
		date("rental_date") as "Alquiler por día"
from rental r 
group by date ("rental_date")
order by count("rental_date") desc;

--24.  Encuentra las películas con una duración superior al promedio.
select "title" as "Películas",
	"length" as "Duración Película"
from "film"
where "length" > (
	select avg("length")
	from film)	;

--25. Averigua el número de alquileres registrados por mes
select extract(year from "rental_date") as "Año",
		extract(month from "rental_date") as "Mes",
		count("rental_date") as "Número Alquileres"
from "rental"
group by extract(year from "rental_date"),
		extract(month from "rental_date")
order by "Año";

--26. Encuentra el promedio, la desviación estándar y varianza del total pagado.
select round(AVG("amount"),2) as "Promedio Total Pagado",
		round(variance("amount"),2) as "Varianza del Total Pagado",
		round(stddev("amount"),2) as "Desviación Estándar"
from "payment";

--27. ¿Qué películas se alquilan por encima del precio medio?
--payment se une con renta por rental_id, rental se une con inventory inventory_id , inventory se une con film "film_id"
select f."title" as "Películas",
		p."amount" as "Precio Alquiler"
from "film" f
inner join inventory i 
on f."film_id" = i."film_id"
inner join rental r
on r."inventory_id" = i."inventory_id"
inner join payment p
on p."rental_id" = r."rental_id"
where p."amount" >(
	select round(avg(p."amount"),2)
	from "payment" p);

--28.Muestra el id de los actores que hayan participado en más de 40 películas.
select fa."actor_id" as "ID Actores",
	count(f."film_id") as "Películas",
	concat (a."first_name", ' ',a."last_name") as "Actor"
from film f
inner join "film_actor" fa
on fa. "film_id" = f. "film_id"
inner join "actor"  a
on fa. "actor_id" = a."actor_id"
group by fa."actor_id", a."first_name", a."last_name"
having count(f."film_id") > 40;

--29.Obtener todas las películas y, si están disponibles en el inventario, mostrar la cantidad disponible.
select f."title" as "Película",
		count(i. "inventory_id") as "Inventario Disponible"
from "film" f
inner join inventory i 
on f."film_id" = i."film_id"
left join rental r 
on i."inventory_id" = r."inventory_id"
  and r."return_date" is null
where r."rental_id" is null
group by f."title"
order by "Inventario Disponible" desc;

--30. Obtener los actores y el número de películas en las que ha actuado.
select concat (a."first_name", ' ',a."last_name") as "Actor",
		count(fa2."film_id") as "Películas"
from film_actor fa2 
inner join "actor"  a
on fa2. "actor_id" = a."actor_id"
group by a."first_name",a."last_name"
order by count (fa2."film_id");

--31.Obtener todas las películas y mostrar los actores que han
--actuado en ellas, incluso si algunas películas no tienen actores asociados.
select f. "title" as "Película",
	concat (a."first_name", ' ',a."last_name") as "Actor"
from "film" f 
left join "film_actor" fa 
on f."film_id" = fa."film_id"
left join "actor" a 
on a."actor_id" = fa."actor_id"
order by f."title";

--32. Obtener todos los actores y mostrar las películas en las 
--que han actuado, incluso si algunos actores no han actuado en ninguna película.
select concat (a."first_name", ' ',a."last_name") as "Actor",
		f. "title" as "Película"
from "actor" a 
left join "film_actor" fa 
on a."actor_id" = fa."actor_id"
left join "film" f 
on fa."film_id" = f."film_id"
order by "Actor";

--33. Obtener todas las películas que tenemos y todos los registros de alquiler.
select f."title" as "Peliculas",
		r."rental_id" as "ID Alquiler",
		r."rental_date" as "Fecha Alquiler"
from "film" f
left join inventory i 
on f."film_id" = i."film_id"
left join rental r 
on i."inventory_id" = r."inventory_id"
order by f."title";

--34. Encuentra los 5 clientes que más dinero se hayan gastado con nosotros.
select concat(c."first_name", ' ', c."last_name") as "Clientes",
		c.customer_id,
		sum(p."amount") as "Importe Compra"
from customer c 
inner join payment p
on p.customer_id = c.customer_id
group by c."first_name", c."last_name", c.customer_id
order by sum(p."amount") desc
limit 5;

--35.Selecciona todos los actores cuyo primer nombre es 'Johnny'.
select "actor_id",
		"first_name"
from actor a 
where "first_name" = 'JOHNNY';

--36.Renombra la columna “first_nameˮ como Nombre y “last_nameˮ como Apellido.
select "first_name" as "Nombre",
		"last_name" as "Apellido"
from actor a ;

--37.Encuentra el ID del actor más bajo y más alto en la tabla actor.
select MIN("actor_id") as "Menor ID",
		MAX("actor_id") as "Mayor ID"
from "actor" a;

--38.Cuenta cuántos actores hay en la tabla “actorˮ.
select COUNT("actor_id") 
from "actor" a;

--39.Selecciona todos los actores y ordénalos por apellido en orden ascendente.
select "first_name" as "Nombre",
		"last_name" as "Apellido" 
from "actor" a
order by "Apellido" asc;

--40. Selecciona las primeras 5 películas de la tabla “filmˮ.
select "title" as "Peliculas"
from "film"
limit 5;

--41.Agrupa los actores por su nombre y cuenta cuántos actores tienen el mismo nombre. ¿Cuál es el nombre más repetido?
select "first_name" as "Nombre Actores",
		count("first_name") as "Nombre Repetido"
from actor a 
group by "first_name"
order by count("first_name");
	--nombre más repetido
select "first_name" as "Nombre Actores",
		count("first_name") as "Nombre Repetido"
from actor a 
group by "first_name"
order by count("first_name") desc
limit 1 ;

--42. Encuentra todos los alquileres y los nombres de los clientes que los realizaron.
select r."rental_id" as "Alquiler",
		concat(c."first_name", ' ',c."last_name") as "Nombre Clientes"
from rental r 
inner join customer c 
on r.customer_id = c.customer_id
order by r."rental_id";

--43. Muestra todos los clientes y sus alquileres si existen, incluyendo aquellos que no tienen alquileres.
select concat(c."first_name", ' ',c."last_name") as "Nombre Clientes",
		r."rental_id" as "Alquiler"
from customer c 
left join rental r 
on r.customer_id = c.customer_id
order by "Alquiler" ;

--44.Realiza un CROSS JOIN entre las tablas film y category. 
--¿Aporta valor esta consulta? ¿Por qué? Deja después de la consulta la contestación.
select *
from film f 
cross join category c ;
--Respuesta: no aporta valor porque no podemos entender la pelicula y su categoría, solo muestra todos los resultados sin
		--nada de relación entre si

--45. Encuentra los actores que han participado en películas de la categoría 'Action'
	--actor con film_actor en "actor_id", film_actor con film en "film_id", film con film_category en "category_id" con category
select concat(a."first_name", ' ', a."last_name") as "Actor",
		c."name" as "Categoría"
from actor a 
inner join "film_actor" fa
on a."actor_id" = fa."actor_id"
inner join "film_category" fc
on fa."film_id" = fc."film_id"
inner join "category" c 
on c."category_id" = fc."category_id"
where c."name" = 'Action';

--46.Encuentra todos los actores que no han participado en películas.
select fa."film_id",
		concat(a."first_name", ' ', a."last_name") as "Actor"
from "actor"a
left join film_actor fa 
on a.actor_id = fa.actor_id
where fa."film_id" is null; 

--47.Selecciona el nombre de los actores y la cantidad de películas en las que han participado
select concat(a."first_name", ' ', a."last_name") as "Actor",
		count(fa."film_id") as "Peliculas Realizadas"
from "actor"a
left join film_actor fa 
on a.actor_id = fa.actor_id
group by a."actor_id", a."first_name", a."last_name"
order by "Peliculas Realizadas" asc;

--48. Crea una vista llamada “actor_num_peliculasˮ que 
--muestre los nombres de los actores y el número de películas en las que han participado.
create view "actor_num_peliculas" as
select concat(a."first_name", ' ', a."last_name") as "Actor",
		count(fa."film_id") as "Peliculas Realizadas"
from "actor"a
left join film_actor fa 
on a.actor_id = fa.actor_id
group by a."actor_id", a."first_name",a."last_name" ;

select *
from "actor_num_peliculas";

--49. Calcula el número total de alquileres realizados por cada cliente.
select "customer_id" as "ID Clientes",
		count("rental_date") as "Alquiler por Cliente"
from rental r
group by "customer_id" 
order by "customer_id" asc;

--50.Calcula la duración total de las películas en la categoría 'Action'.
select c. "name" as "Categoría",
		sum(f."length") as "Duración Total"
from "film" f
inner join "film_category" fc
on f."film_id"  = fc."film_id"
inner join "category" c
on c."category_id" = fc."category_id"
where c."name" = 'Action'
group by c."name";

--51. Crea una tabla temporal llamada “cliente_rentas_temporalˮ para almacenar el total de alquileres por cliente.
create temporary table “cliente_rentas_temporalˮ as
select "customer_id" as "ID Clientes",
		count("rental_date") as "Alquiler por Cliente"
from "rental" r
group by "customer_id";

select *
from “cliente_rentas_temporalˮ;

--52. Crea una tabla temporal llamada “peliculas_alquiladasˮ que almacene las 
	--películas que han sido alquiladas al menos 10 veces.
create temporary table "peliculas_alquiladas" as
select f."title" as "Película",
		count(r."rental_date") as "Alquiler por Cliente"
from "rental" r
inner join "inventory" i
on r."inventory_id" = i. "inventory_id"
inner join "film" f
on f."film_id" = i. "film_id"
group by f."title"
having count("rental_date") >= 10;

select * 
from "peliculas_alquiladas";

--53. Encuentra el título de las películas que han sido alquiladas por el cliente con el nombre ‘Tammy Sandersʼ 
	--y que aún no se han devuelto. Ordena los resultados alfabéticamente por título de película.
--customer y rental "customer_id", rental e inventory "inventory_id", film e inventory "film_id"
select f."title" as "Película",
		concat(c."first_name", ' ',c."last_name") as "Nombre Cliente"
from "film" f 
inner join "inventory" i 
on f."film_id" = i."film_id"
inner join "rental"r
on r."inventory_id" = i."inventory_id"
inner join "customer" c
on r.customer_id = c.customer_id
where r.return_date is null and concat(c."first_name", ' ',c."last_name") = 'TAMMY SANDERS'
order by f."title" asc;

--54.Encuentra los nombres de los actores que han actuado en al menos una película que pertenece a 
	--la categoría ‘Sci-Fiʼ. Ordena los resultados alfabéticamente por apellido.
select concat (a."first_name", ' ', a."last_name") as "Actor",
		c."name" as "Categoría"
from "actor" a
inner join "film_actor" fa 
on a."actor_id" = fa."actor_id"
inner join film_category fc 
on fa."film_id" = fc."film_id"
inner join category c 
on fc."category_id" = c."category_id"
where c."name" = 'Sci-Fi'
group by a."actor_id", c."name", a."first_name", a."last_name"
order by a."last_name" ASC;

--55.Encuentra el nombre y apellido de los actores que han actuado en películas que se alquilaron 
	--después de que la película ‘Spartacus Cheaperʼ se alquilara por primera vez. 
	--Ordena los resultados alfabéticamente por apellido.
--actor con film_actor "actor_id", film_actor con inventory "film_id", rental con inventory "inventory_id"
select a."first_name" as "Nombre Actor",
		a."last_name" as "Apellido Actor"
from "actor" a
inner join "film_actor" fa
on a.actor_id  = fa.actor_id
inner join "film" f
on f."film_id" = fa."film_id"
inner join "inventory" i
on i.film_id  = fa.film_id
inner join "rental" r
on  r."inventory_id" = i. "inventory_id"
where "rental_date" > (
--ahora quiero film con inventory de nuevo y rental
	select min(r1."rental_date")
	from "film" f1
	inner join "inventory" i1
	on i1."film_id"= f1."film_id"
	inner join "rental" r1
	on i1. "inventory_id" = r1. "inventory_id"
	where f1."title" = 'SPARTACUS CHEAPER')
group by a."first_name", a."last_name" 
order by a."last_name";
	

--56. Encuentra el nombre y apellido de los actores que no han actuado en ninguna película de la categoría ‘Musicʼ.
select a."first_name" as "Nombre Actor",
		a."last_name" as "Apellido Actor"
from "actor" a
where a."actor_id" not in (
	select fa."actor_id"
	from "film_actor" fa
	inner join "film_category" fc
	on fc."film_id" = fa."film_id"
	inner join "category" c
	on fc."category_id" = c."category_id"
	where c."name" = 'Music');

--57.Encuentra el título de todas las películas que fueron alquiladas por más de 8 días.
select f."title" as "Película",
  date_part('day', r."return_date" - r."rental_date") as "Dias Alquilado"
from film f
inner join inventory i on f."film_id" = i."film_id"
inner join rental r on i."inventory_id" = r."inventory_id"
where r."return_date" - r."rental_date" > interval '8 days'
order by f."title";

--58. Encuentra el título de todas las películas que son de la misma categoría que ‘Animationʼ.
select f. "title" as "Película",
		c."name" as "Categoría"
from "film"f
inner join "film_category" fc
on f."film_id" = fc. "film_id"
inner join "category" c
on fc."category_id"  = c."category_id"
where c."category_id" =  (
	select c1."category_id" 
	from "category" c1
	where c1."name" = 'Animation')
order by f."title";

--59. Encuentra los nombres de las películas que tienen la misma duración que la película con el título ‘Dancing Feverʼ. 
	--Ordena los resultados alfabéticamente por título de película.
select "title" as "Película",
		"length" as "Duración"
from film f 
where "length" = (
		select "length"
		from film f2 
		where title = 'DANCING FEVER')
order by "title" asc;

--60. Encuentra los nombres de los clientes que han alquilado al menos 7 películas distintas. 
--Ordena los resultados alfabéticamente por apellido.
select concat(c."first_name", ' ', c."last_name") as "Cliente"
from "customer" c 
inner join "rental" r
on r."customer_id" = c."customer_id"
inner join "inventory" i
on r."inventory_id"  = i. "inventory_id"
inner join "film" f
on i."film_id" = f."film_id"
group by c."customer_id", c."first_name", c."last_name"
having count(distinct f."film_id")>= 7
order by c."last_name";


--61. Encuentra la cantidad total de películas alquiladas por categoría y 
--muestra el nombre de la categoría junto con el recuento de alquileres.
select c. "name" as "Categorías",
		count(r."rental_id") as "Cantidad Películas Alquildas"
from "category" c
inner join "film_category" fc
on fc."category_id" = c."category_id"
inner join "film" f
on f.film_id = fc.film_id
inner join "inventory" i
on i."film_id" = f."film_id"
inner join "rental" r
on r."inventory_id" = i."inventory_id" 
group by c."name";

--62. Encuentra el número de películas por categoría estrenadas en 2006
select  c. "name" as "Categoría",
		count(f."title") as "Estreno 2006"
from "category" c
inner join "film_category" fc
on fc."category_id" = c."category_id"
inner join "film" f
on f."film_id" = fc."film_id"
where f."release_year" = 2006
group by c."name";

--63. Obtén todas las combinaciones posibles de trabajadores con las tiendas que tenemos.
select *
from staff s 
cross join store s2 ;

--64.Encuentra la cantidad total de películas alquiladas por cada cliente y muestra el ID del cliente, 
--su nombre y apellido junto con la cantidad de películas alquiladas.
select c."customer_id" as "ID Cliente",
		concat(c."first_name", ' ', c."last_name") as "Nombre Cliente",
  		count(r."rental_id") as "Cantidad Alquilada"
from "customer" c
inner join "rental" r 
on c."customer_id" = r."customer_id"
group by c.customer_id, c.first_name, c.last_name
order by count(r."rental_id");



