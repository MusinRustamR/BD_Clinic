CREATE TABLESPACE med_space location '/home/tabspace';

CREATE database MedClinic  TABLESPACE med_space;

CREATE SCHEMA employee_data ;
CREATE SCHEMA client_data ;
CREATE SCHEMA adress ;
CREATE SCHEMA clinic_data ;


CREATE TABLE IF NOT EXISTS clinic_data.appointment
(
	"Id" integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "employee_Id" integer NOT NULL,
    "client_Id " integer NOT NULL,
    date timestamp with time zone NOT NULL,
    duration interval,
    comment text COLLATE pg_catalog."default",
    "isVHI" boolean,
    diagnosis_id integer NOT NULL,
    is_primary boolean NOT NULL,
    CONSTRAINT appointment_pkey PRIMARY KEY ("Id")
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS clinic_data.appointment_treatment
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    appoitment_id integer NOT NULL,
    treatment_id integer NOT NULL,
    CONSTRAINT appointment_treatment_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS client_data.client
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    firstname character varying(100) COLLATE pg_catalog."default" NOT NULL,
    lastname character varying(100) COLLATE pg_catalog."default" NOT NULL,
    middlename character varying(100) COLLATE pg_catalog."default",
    phone character varying(12) COLLATE pg_catalog."default" NOT NULL,
    adress_id integer NOT NULL,
    birthdate date NOT NULL,
    vhi_id integer NOT NULL,
    CONSTRAINT client_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS client_data.client_adress
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    street_id integer NOT NULL,
    house integer NOT NULL,
    house_character character varying(5) COLLATE pg_catalog."default",
    entrace integer,
    flat integer,
    CONSTRAINT client_adress_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS clinic_data.department
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    "Name" character varying(255) COLLATE pg_catalog."default" NOT NULL,
    head_doctor_id integer NOT NULL,
    CONSTRAINT department_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS clinic_data.diagnosis
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    appointment_id integer NOT NULL,
    name text COLLATE pg_catalog."default" NOT NULL,
    description text COLLATE pg_catalog."default",
    CONSTRAINT diagnosis_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS adress.district
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    name character varying(255) COLLATE pg_catalog."default",
    CONSTRAINT district_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS employee_data.employee
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    firstname character varying(100) COLLATE pg_catalog."default" NOT NULL,
    middlename character varying(100) COLLATE pg_catalog."default",
    lastname character varying(100) COLLATE pg_catalog."default" NOT NULL,
    birthdate date NOT NULL,
    phone character varying(12) COLLATE pg_catalog."default" NOT NULL,
    email character varying(255) COLLATE pg_catalog."default" NOT NULL,
    departmnet_id integer,
    CONSTRAINT employee_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS employee_data.employee_speciality
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    speciality_id integer NOT NULL,
    employee_id integer NOT NULL,
    CONSTRAINT employee_speciality_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS employee_data.speciality
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    name character varying(150) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT speciality_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS adress.street
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    name character varying(255) COLLATE pg_catalog."default" NOT NULL,
    district_id integer,
    CONSTRAINT street_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS clinic_data.treatment
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    name text COLLATE pg_catalog."default" NOT NULL,
    cost numeric(10, 2),
    treatment_type_id integer NOT NULL,
    CONSTRAINT treatment_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS clinic_data.treatment_type
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    name character varying(255) COLLATE pg_catalog."default" NOT NULL,
    CONSTRAINT treatment_type_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

CREATE TABLE IF NOT EXISTS client_data.vhi
(
    id integer NOT NULL GENERATED ALWAYS AS IDENTITY ( INCREMENT 1 START 1 MINVALUE 1 MAXVALUE 2147483647 CACHE 1 ),
    cert_numb character varying(100) COLLATE pg_catalog."default" NOT NULL,
    insurer_name character varying(255) COLLATE pg_catalog."default" NOT NULL,
    startdate time with time zone NOT NULL,
    enddate time with time zone NOT NULL,
    CONSTRAINT vhi_pkey PRIMARY KEY (id)
)
TABLESPACE med_space
;

ALTER TABLE IF EXISTS clinic_data.appointment
ADD CONSTRAINT diag_id FOREIGN KEY (diagnosis_id)
REFERENCES clinic_data.diagnosis (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS clinic_data.appointment
ADD CONSTRAINT emp_id FOREIGN KEY ("employee_Id")
REFERENCES employee_data.employee (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS clinic_data.appointment_treatment
ADD CONSTRAINT appointment_id FOREIGN KEY (appoitment_id)
REFERENCES clinic_data.appointment (Id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS clinic_data.appointment_treatment
ADD CONSTRAINT treatment_id FOREIGN KEY (treatment_id)
REFERENCES clinic_data.treatment (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;

ALTER TABLE IF EXISTS clinic_data.diagnosis
ADD CONSTRAINT appointment_id FOREIGN KEY (appointment_id)
REFERENCES  clinic_data.appointment (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS client_data.client
ADD CONSTRAINT adress_id FOREIGN KEY (adress_id)
REFERENCES client_data.client_adress (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS client_data.client
ADD CONSTRAINT vhi_id FOREIGN KEY (vhi_id)
REFERENCES client_data.vhi (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS client_data.client_adress
ADD CONSTRAINT street_id FOREIGN KEY (street_id)
REFERENCES adress.street (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS clinic_data.department
ADD CONSTRAINT head_doctor_id FOREIGN KEY (head_doctor_id)
REFERENCES employee_data.employee (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;

ALTER TABLE IF EXISTS employee_data.employee
ADD CONSTRAINT departmnet_id FOREIGN KEY (departmnet_id)
REFERENCES clinic_data.department (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS employee_data.employee_speciality
ADD CONSTRAINT employee_id FOREIGN KEY (employee_id)
REFERENCES employee_data.employee (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS employee_data.employee_speciality
ADD CONSTRAINT spec_id FOREIGN KEY (speciality_id)
REFERENCES employee_data.speciality (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS adress.street
ADD CONSTRAINT district_id FOREIGN KEY (district_id)
REFERENCES adress.district (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


ALTER TABLE IF EXISTS clinic_data.treatment
ADD CONSTRAINT treatment_type_id FOREIGN KEY (treatment_type_id)
REFERENCES clinic_data.treatment_type (id) MATCH SIMPLE
ON UPDATE NO ACTION
ON DELETE NO ACTION;


