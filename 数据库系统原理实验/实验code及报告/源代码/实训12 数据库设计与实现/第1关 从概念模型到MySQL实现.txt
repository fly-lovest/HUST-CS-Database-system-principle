# 请将你实现flight_booking数据库的语句写在下方：
create database flight_booking;
use flight_booking;
create table user(
	user_id int primary key auto_increment,
	firstname varchar(50) not null,
	lastname varchar(50) not null,
	dob date not null,
	sex char(1) not null,
	email varchar(50),
	phone varchar(30),
	username varchar(20) not null unique,
	password char(32) not null,
	admin_tag tinyint not null default 0
);

create table passenger(
	passenger_id int primary key auto_increment,
	id char(18) not null unique,
	firstname varchar(50) not null,
	lastname varchar(50) not null,
	mail varchar(50),
	phone varchar(20) not null,
	sex char(1) not null,
	dob date
);

create table airport(
	airport_id int primary key auto_increment,
	iata char(3) not null unique,
	icao char(4) not null unique,
	name varchar(50) not null,
	city varchar(50),
	country varchar(50),
	latitude decimal(11,8),
	longitude decimal(11,8),
	index(name)
);

create table airline(
	airline_id int primary key auto_increment,
	name varchar(30) not null,
	iata char(2) not null unique,
	airport_id int not null,
	constraint FK_airline_airport foreign key (airport_id) references airport(airport_id)
);

create table airplane(
	airplane_id int primary key auto_increment,
	type varchar(50) not null,
	capacity smallint not null,
	identifier varchar(50) not null,
	airline_id int not null,
	constraint FK_airplane_airline foreign key (airline_id) references airline(airline_id)
);

create table flightschedule(
	flight_no char(8) primary key,
	departure time not null,
	arrival time not null,
	duration smallint not null,
	monday tinyint default 0,
	tuesday tinyint default 0,
	wednesday tinyint default 0,
	thursday tinyint default 0,
	friday tinyint default 0,
	saturday tinyint default 0,
	sunday tinyint default 0,
	airline_id int not null,
	`from` int not null,
	`to` int not null,
	constraint FK_flightschedule_airline foreign key (airline_id) references airline(airline_id),
	constraint FK_flightschedule_airport1 foreign key (`from`) references airport(airport_id),
	constraint FK_flightschedule_airport2 foreign key (`to`) references airport(airport_id)
);

create table flight(
	flight_id int primary key auto_increment,
	departure datetime not null,
	arrival datetime not null,
	duration smallint not null,
	airline_id int not null,
	airplane_id int not null,
	flight_no char(8) not null,
	`from` int not null,
	`to` int not null,
	constraint FK_flight_airline foreign key (airline_id) references airline(airline_id),
	constraint FK_flight_airplane foreign key (airplane_id) references airplane(airplane_id),
	constraint FK_flight_flightschedule foreign key (flight_no) references flightschedule(flight_no),
	constraint FK_flight_airport1 foreign key (`from`) references airport(airport_id),
	constraint FK_flight_airport2 foreign key (`to`) references airport(airport_id)
);

create table ticket(
	ticket_id int primary key auto_increment,
	seat char(4),
	price decimal(10,2) not null,
	flight_id int not null,
	passenger_id int not null,
	user_id int not null,
	constraint FK_ticket_flight foreign key (flight_id) references flight(flight_id),
	constraint FK_ticket_passenger foreign key (passenger_id) references passenger(passenger_id),
	constraint FK_ticket_user foreign key (user_id) references user(user_id)
);