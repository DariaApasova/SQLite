# SQLite
#	Запрос на самую популярную услугу в месяце.
'select title as Услуга, count(*) as Количество from services s'
join services_visits_workers svw on s.id=svw.id_service
join visits v on v.id=svw.id_visit
where v.date_visit between "2021-08-01" and "2021-08-31"
group by s.title
order by count(*) desc 
limit 1;
#	Запрос на ТОП-10 дорогих заказов 
select id as Визит, price as Стоимость from visits
group by id
order by price desc
limit 10;
#	Запрос на количество заказов мастеров
select name as Мастер, count(*) as Количество_заказов from workers w
join services_visits_workers svw on w.id=svw.id_worker
join visits v on v.id=svw.id_visit
where v.date_visit between "2021-08-01" and "2021-08-31"
group by w.name 
order by count(*) desc;
#	Запрос на все заказы, в которых не принимал участие определенный мастер 
select w.name as Работник, v.id as Номер_визита
from workers as w
inner join services_visits_workers svw on w.id=svw.id_worker
left join visits as v on v.id=svw.id_visit
where w.id not in (9);
#	Все Заказы, где номера телефонов клиентов начинаются на…
select v.id as Номер_визита, c.name as Клиент, c.phone as Мобильный from visits as v
join clients as c on c.id=v.client_id
where c.phone like '+7(999)%';
#	Заказы, где присутствует определенная услуга
select v.id as Номер_визита, s.title as Услуга  from visits as v
inner join services_visits_workers svw on v.id=svw.id_visit
join services s on s.id=svw.id_service
where s.title  in ("Коррекция бровей");
#	Запрос на услуги, которые не использовались в определенном месяце
select s.title as Услуга 
from services s 
where not exists( select svw.id_service from services_visits_workers svw join visits v where v.date_visit between "2021-08-01" and "2021-08-31" and svw.id_service=s.id)
group by s.title;
