## System Architecture
Below is the C4 Level 2 diagram rendered directly from DSL.

```kroki-structurizr

workspace "CATSALUT" "Sistemes ARGOS i HES (Quique)" {
    
    model {
    
        group "Serveis Externs Core"{
            cdrRdc = softwaresystem "CDR/RDC" "Repositori Dades Clíniques. Plataforma de gestió del coneixement clínic d'acord amb l'estàndard openEHR" "External system"
            mpi = softwaresystem "MPI" "Mestre de Pacients segons l'estàndard FHIR R4" "External system"
            servidorTerminologic = softwaresystem "SERVIDOR TERMILOGIC (IS3)" " Repositori de catàlegs terminològics segons estàndard FHIR R4" "External system"
            pds = softwaresystem "PDS" "Plataforma de Seguretat. Proveïdor transversal d'identitat i accés, basat en tokens OpenId Connect o SAML, i compatible amb models RBAC/ABAC d'autorització" "External system"
            plataformaDeAuditoria = softwaresystem "PAUD" "Plataforma de Auditoria. Component transversal d'auditoria segons l'estàndard ATNA FHIR AuditEvent" "External system"
            eventhub = softwaresystem "EVENTHUB" "Plataforma Eventhub. Broker transversal de missatgeria asíncrona sobre Kafka Confluent, per donar suport a dissenys d'arquitectura basats en esdeveniments" "External system"
            sapArgos = softwaresystem "SAP ARGOS" "HIS de l'ICS basat en SAP" "External system"
            hc3 = softwaresystem "HC3" "Història Clínica Compartida de Catalunya" "External system"
            gicar = softwaresystem "GICAR" "Gestió d’identitats i control d’accés als recursos de persones que interactuen amb els sistemes d’informació." "External system"
            ecap = softwaresystem "ECAP" "Sistema d'informació per Atenció Primària" "External system"
            sire = softwaresystem "SIRE" "Sistema de recepta electrònica" "External system"
            formularisHes = softwaresystem "FHES" "Habilitador de Formularis del HES" "External system"
            lms = softwaresystem "LMS" "Portal d'aplicacions de Salut per al ciutadà" "External system"
            rgvArgos = softwaresystem "RGV ARGOS" "Registre de Variables Clíniques i laboratori" "External system"
        }
        
 
        
        group "Sistema ARGOS"{
            cercadorDiagnosticsDb = softwaresystem "CERCADOR DIAGNÒSTICS DB" "ElasticSearch" "Database,ElasticSearch"
            cercadorDiagnostics = softwaresystem "CERCADOR DIAGNÒSTICS" "Cercador de diagnòstics basat en tecnologia Lucent" "Information system"{
                tags "APP:CERCADOR DIAGNÒSTICS"
                -> cercadorDiagnosticsDb {
                    tags "DatabaseRelation"
                }
                 -> sapArgos {
                    tags "ExternalServiceRelation"
                 }
            }
            
            argostecaDb = softwaresystem "ARGOSTECA DB" "MySQL" "Database,MySql"
            argosteca = softwaresystem "ARGOSTECA" "Plataforma reproducció vídeos en Streaming" "Information system"{
                 tags "APP:ARGOSTECA"
                -> argostecaDb {
                    tags "DatabaseRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
            gestioVisites =  softwaresystem "GESTIÓ VISITES" "Enviar un SMS que contindrà un enllaç" "Information system"{
                tags "APP:GESTIÓ VISITES"
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
            rgvDb = softwaresystem "RGV DB" "MongoDb" "Database,MongoDb"
            rgv = softwaresystem "RGV" "Registre de Variables Clíniques i laboratori" "Information system" {
                tags "APP:REGISTRE DE VARIABLES CLÍNIQUES I LABORATORI"
                -> rgvDb {
                    tags "DatabaseRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            
            etcDb = softwareSystem "ETC DB" "MongoDB" "Database,MongoDb"
            etcRedisDb = softwareSystem "ETC DB REDIS" "Redis" "Database,Redis"
            etc = softwareSystem "ETC" "Estació de Treball Clínica de Argos" "Information system"{
                tags "APP:ESTACIÓ DE TREBALL CLÍNICA DE ARGOS"
                -> etcDb {
                    tags "DatabaseRelation"
                }
                -> etcRedisDb {
                    tags "DatabaseRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
             etinf = softwareSystem "ETINF" "Estació Treball d'Infermeria de Argos" "Information system" {
                tags "APP:ESTACIÓ TREBALL D'INFERMERIA DE ARGOS"
                -> rgvArgos {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
            etpatDb = softwareSystem "ETEPAT DB" "PostgrSQL" "Database, Postgres"
            etpatDb2 = softwareSystem "ETEPAT DB 2" "ElasticSearch" "Database, ElasticSearch"
            etpat = softwareSystem "ETPAT" "Sistema d’Informació d’Anatomia Patològica" "Information system"{
                tags "APP:SISTEMA D’INFORMACIÓ D’ANATOMIA PATOLÒGICA"
                -> etpatDb {
                    tags "DatabaseRelation"
                }
                -> etpatDb2 {
                    tags "DatabaseRelation"
                }
                -> rgvArgos {
                    tags "ExternalServiceRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
            publicadorDates = softwaresystem "PUBLICADOR DADES" "Publicar variables clíniques i escales funcionals a HC3 a partir de les dades registrades prèviament a RGV" "Information system"{
                tags "APP:PUBLICADOR DADES"
                -> rgvArgos {
                    tags "ExternalServiceRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }    
                -> hc3 {
                    tags "ExternalServiceRelation"
                }    
            }
            
            albaDb = softwareSystem "ALBA DB" "MongoDbAtlas" "Database,MongoDbAtlas"
            alba = softwaresystem "ALBA" "Entorn descriptiu, diagnòstic, predictiu i prescriptiu de lactivitat quirúrgica hospitalària" "Information system"{
                tags "APP:ALBA"
                -> albaDb {
                    tags "DatabaseRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
            }
            
            etpatSiscatDb = softwaresystem "ETPAT_SISCAT DB" "PostgreSQL" "Database,Postgres"
            etpatSiscat = softwaresystem "ETPAT SISCAT" "Sistema d’informació per la coordinació de les actuacions de salut laboral" "Information system"{
                tags "APP:ETPAT SISCAT"
                -> etpatSiscatDb {
                    tags "DatabaseRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> rgvArgos {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                   
            }
            risSiscatDb = softwaresystem "RIS_SISCAT DB" "SqlServer" "Database,SqlServer"
            risSiscat = softwaresystem "RIS SISCAT" "RIS de serveis de radiologia i medicina nuclear" "Information system"{
                tags "APP:RIS SISCAT"
                -> risSiscatDb {
                    tags "DatabaseRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }   
            }
            
        }
        
        group "Sistema HES" {
        
            hesCursClinicDb = softwareSystem "HES CURS CLÍNIC DB" "MongoDbAtlas" "Database,MongoDbAtlas"
            hesCursClinic = softwareSystem "CURS CLÍNIC" "Nova Estació de treball per professionals assistencials" "Information system" {
                tags "APP:CURS CLÍNIC"
                -> hesCursClinicDb {
                    tags "DatabaseRelation"
                }
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> sapArgos {
                    tags "ExternalServiceRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesIntegradorImatgesDb = softwaresystem "HES INTEGRADOR D'IMATGES DB" "Oracle" "Database,Oracle"
            hesIntegradorImatges = softwaresystem "HES INTEGRADOR D'IMATGES" "Capturar un conjunt d’imatges i associar-les a una cita" "Information system"{
                tags "APP:INTEGRADOR D'IMATGES"
                -> hesIntegradorImatgesDb {
                    tags "DatabaseRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            hesServidorTerminologicDb = softwaresystem "HES SERVIDOR TERMINOLÒGIC DB" "PostgreSQL" "Database,Postgres"
            hesServidorTerminologic = softwaresystem "HES SERVIDOR TERMINOLÒGIC" "Gestiona, manté i permet consultar els catàlegs de dades clíniques i no clíniques" "Information system"{
                tags "APP:SERVIDOR TERMINOLÒGIC"
                -> hesServidorTerminologicDb {
                    tags "DatabaseRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesPlataformaAuditoriaDb = softwaresystem "HES PLATAFORMA AUDITORIA DB" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesPlataformaAuditoria = softwaresystem "HES PLATAFORMA AUDITORIA" "HES - Plataforma d'auditoria" "Information system"{
                tags "APP:PLATAFORMA AUDITORIA"
                -> hesPlataformaAuditoriaDb {
                    tags "DatabaseRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesCercadorDiagnosticsDb = softwaresystem "HES CERCADOR DIAGNÒSTICS DB" "Oracle" "Database,Oracle"
            hesCercadorDiagnostics = softwaresystem "HES CERCADOR DIAGNÒSTICS" "Cercador de diagnòstics del HES" "Information system" {
                tags "APP:CERCADOR DIAGNÒSTICS"
                -> hesCercadorDiagnosticsDb {
                    tags "DatabaseRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesFormularisDb1 = softwaresystem "HES FORMULARIS DB1" "Oracle" "Database,Oracle"
            hesFormularisDb2 = softwaresystem "HES FORMULARIS DB2" "PostgreSQL" "Database,Postgres"
            hesFormularis = softwaresystem "HES FORMULARIS" "Aplicació que permeti crear formularis web de forma dinàmica. Així com permetre la governança de publicació de nous formularis" "Information system"{
                tags "APP:FORMULARIS"
                -> hesFormularisDb1 {
                    tags "DatabaseRelation"
                }
                -> hesFormularisDb2 {
                    tags "DatabaseRelation"
                }
                -> servidorTerminologic  {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesLandingPageDb = softwaresystem "HES LANDING PAGE DB" "PostgreSQL" "Database,Postgres"
            hesLandingPage = softwaresystem "HES LANDING PAGE" "Plana d'inici integrada en ECAP" "Information system"{
                tags "APP:LANDING PAGE"
                -> hesLandingPageDb {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
                -> formularisHes {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesServidorRecursosDb = softwaresystem "HES SERVIDOR RECURSOS DB" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesServidorRecursos = softwaresystem "HES SERVIDOR RECURSOS" "HES - Servidor de recursos" "Information system"{
                tags "APP:SERVIDOR RECURSOS"
                -> hesServidorRecursosDb {
                    tags "DatabaseRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesPlansAccioAdministratiusDb1 = softwaresystem "HES PLANS ACCIÓ ADMINISTRATIUS DB1" "MongoDbAtlas" "Database,MongoDbAtlas"
            hesPlansAccioAdministratiusDb2 = softwaresystem "HES PLANS ACCIÓ ADMINISTRATIUS DB2" "AmazonElasticCache" "Database,AmazonElasticCache"
            hesPlansAccioAdministratius = softwaresystem "HES PLANS ACCIÓ ADMINISTRATIUS" "HES - Plans d'acció administratius" "Information system" {
                tags "APP:PLANS ACCIÓ ADMINISTRATIUS"
                -> hesPlansAccioAdministratiusDb1 {
                    tags "DatabaseRelation"
                }
                -> hesPlansAccioAdministratiusDb2 {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }       
            
            hesCentreAjudaDb1 = softwaresystem "HES CENTRE D'AJUDA I MOTOR DE CERCA DB1" "MongoDB Atlas" "Database, MongoDbAtlas"
            hesCentreAjudaDb2 = softwaresystem "HES CENTRE D'AJUDA I MOTOR DE CERCA DB2" "ElasticSearch" "Database, ElasticSearch"
            hesCentreAjuda = softwaresystem "HES CENTRE D'AJUDA I MOTOR DE CERCA" "HES - Centre d'ajuda i motor de cerca" "Information system" {
                tags "APP:CENTRE D'AJUDA I MOTOR DE CERCA"
                -> hesCentreAjudaDb1 {
                    tags "DatabaseRelation"
                }
                -> hesCentreAjudaDb2 {
                    tags "DatabaseRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                
            }
            
            hesCentreConfiguracioDb1 = softwaresystem "HES CENTRE CONFIGURACIÓ DB1" "PostgreSQL" "Database,Postgres"
            hesCentreConfiguracioDb2 = softwaresystem "HES CENTRE CONFIGURACIÓ DB2" "Redis" "Database,Redis"
            hesCentreConfiguracio = softwaresystem "HES CENTRE CONFIGURACIÓ" "HES - Centre de configuració" "Information system" {
                tags "APP:CENTRE CONFIGURACIÓ"
                -> hesCentreConfiguracioDb1 {
                    tags "DatabaseRelation"
                }
                 -> hesCentreConfiguracioDb2 {
                    tags "DatabaseRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesImmunitzacionsDb = softwaresystem "HES IMMUNITZACIONS DB" "PostgreSQL" "Database, Postgres"
            hesImmunitzacions = softwaresystem "HES IMMUNITZACIONS" "HES Immunitzacions" "Information system"{
                tags "APP:IMMUNITZACIONS"
                -> hesImmunitzacionsDb {
                    tags "DatabaseRelation"
                }
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                }
                -> lms {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesVisorDb = softwaresystem "HES VISOR DB" "Oracle" "Database,Oracle"
            hesVisor = softwaresystem "HES VISOR" "Visor del HES" "Information system"{
                tags "APP:VISOR"
                -> hesVisorDb {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
                -> sire {
                    tags "ExternalServiceRelation"
                }
                -> lms {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesEconsultaDb1 = softwaresystem "HES eCONSULTA DB1" "Oracle" "Database,Oracle"
            hesEconsultaDb2 = softwaresystem "HES eCONSULTA DB2" "MongoDb" "Database,MongoDb"
            hesEconulta = softwaresystem "HES eCONSULTA" "HES eConsulta" "Information system"{
                tags "APP:ECONSULTA"
                -> hesEconsultaDb1 {
                    tags "DatabaseRelation"
                }
                -> hesEconsultaDb2 {
                    tags "DatabaseRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> lms {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesCdrDb1 = softwaresystem "HES RDC DB1" "MongoDb" "Database,MongoDb"
            hesCdrDb2 = softwaresystem "HES RDC DB2" "PostgreSQL" "Database,Postgres"
            hesCdr = softwaresystem "HES RDC" "HES RDC" "Information system"{
                tags "APP:RDC"
                -> hesCdrDb1 {
                    tags "DatabaseRelation"
                }
                -> hesCdrDb2 {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> gicar {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesSistemaExpertDb = softwaresystem "HES SISTEMA EXPERT DB" "PostgreSQL" "Database,Postgres"
            hesSistemaExpert = softwaresystem "HES SISTEMA EXPERT" "Nou sistema “expert” de prescripció farmacèutica" "Information system"{
                tags "APP:SISTEMA EXPERT"
                -> hesSistemaExpertDb {
                    tags "DatabaseRelation"
                }
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesEcapDb = softwaresystem "HES ECAP DB" "PostgreSQL" "Database,Postgres"
            hesEcap = softwaresystem "HES ECAP" "Comunicacions amb ECAP" "Information system"{
                tags "APP:ECAP"
                -> hesEcapDb {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
                
            }
            
            hesPrescripcioDb1 = softwaresystem "HES PRESCRIPCIÓ DB1" "PostgreSQL" "Database,Postgres"
            hesPrescripcioDb2 = softwaresystem "HES PRESCRIPCIÓ DB2" "Redis" "Database,Redis"
            hesPrescripcio = softwaresystem "HES PRESCRIPCIÓ " "Prescripció" "Information system"{
                tags "APP:PRESCRIPCIÓ"
                -> hesPrescripcioDb1 {
                    tags "DatabaseRelation"
                }
                -> hesPrescripcioDb2 {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }         
                -> pds   {
                    tags "PdsRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
                -> sire {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesPlataformaSeguretatDb = softwaresystem "HES PLATAFORMA DE SEGURETAT DB" "PostgreSQL" "Database,Postgres"
            hesPlataformaSeguretat = softwaresystem "HES PLATAFORMA DE SEGURETAT" "Plataforma de Seguretat" "Information system"{
               tags "APP:PLATAFORMA DE SEGURETAT PDS"
                -> hesPlataformaSeguretatDb {
                    tags "DatabaseRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }        
                -> gicar {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesPromsDb = softwaresystem "HES PROMs DB" "PostgreSQL" "Database,Postgres"
            hesProms = softwaresystem "HES PROMs" "Patient Reported Outcome Measure" "Information system"{
                tags "APP:PROMS"
                -> hesPromsDb {
                    tags "DatabaseRelation"
                }
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }              
                -> mpi {
                    tags "ExternalServiceRelation"
                }        
                -> pds {
                    tags "PdsRelation"
                }        
                -> eventhub {
                    tags "ExternalServiceRelation"
                }        
                -> formularisHes {
                    tags "ExternalServiceRelation"
                }        
                -> lms {
                    tags "ExternalServiceRelation"
                }        
            }
            
            hesConsentimentInformatDb = softwaresystem "HES CONSENTIMENT INFORMAT DB" "PostgreSQL" "Database,Postgres"
            hesConsentimentInformat = softwaresystem "HES CONSENTIMENT INFORMAT" "Custodia dels documents amb els que els ciutadans donen la seva conformitat davant un procés mèdic" "Information system"{
                tags "APP:CONSENTIMENT INFORMAT"
                -> hesConsentimentInformatDb  {
                    tags "DatabaseRelation"
                }  
                -> mpi {
                    tags "ExternalServiceRelation"
                }        
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }        
                -> eventhub {
                    tags "ExternalServiceRelation"
                }            
                -> lms {
                    tags "ExternalServiceRelation"
                }        
            }
            
            hesProcesObstetricDb = softwaresystem "HES PROBS DB" "MongoDb" "Database,MongoDb"
            hesProcesObstetric = softwaresystem "HES PROBS" "Gestió del procés Obstètric" "Information system"{
                tags "APP:PROCÉS OBSTÈTRIC"
                -> hesProcesObstetricDb {
                    tags "DatabaseRelation"
                }   
                -> cdrRdc  {
                    tags "ExternalServiceRelation"
                }       
                -> mpi {
                    tags "ExternalServiceRelation"
                }       
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }       
                -> eventhub {
                    tags "ExternalServiceRelation"
                }       
                -> hc3 {
                    tags "ExternalServiceRelation"
                }       
                -> formularisHes {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesMpiDb1 = softwaresystem "HES MPI DB1" "Oracle" "Database,Oracle"
            hesMpiDb2 = softwaresystem "HES MPI DB2" "MongoDb" "Database,MongoDb"
            hesMpi = softwaresystem "HES MPI" "Master Patient Index" "Information system"{
                tags "APP:MPI"
                -> hesMpiDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesMpiDb2 {
                    tags "DatabaseRelation"
                }   
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }       
            }
            
            hesVacunesDb = softwaresystem "HES VACUNES DB" "PostgreSQL" "Database,Postgres"
            hesVacunes = softwaresystem "HES VACUNES" "Gestió de Vacunes" "Information system"{
                tags "APP:VACUNES"
                -> hesVacunesDb {
                    tags "DatabaseRelation"
                }   
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }       
                -> servidorTerminologic  {
                    tags "ExternalServiceRelation"
                }
                -> mpi {
                    tags "ExternalServiceRelation"
                }       
                -> pds {
                    tags "PdsRelation"
                }       
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }       
                -> lms {
                    tags "ExternalServiceRelation"
                }       
            }
            
            hesProgramacioMotiusDb1 = softwaresystem "HES PROGRAMACIÓ PER MOTIUS DB1" "PostgreSQL" "Database,Postgres"
            hesProgramacioMotiusDb2 = softwaresystem "HES PROGRAMACIÓ PER MOTIUS DB2" "MongoDb" "Database,MongoDb"
            hesProgramacioMotius = softwaresystem "HES PROGRAMACIÓ PER MOTIUS" "Programació per Motius" "Information system"{
                tags "APP:PROGRAMACIÓ PER MOTIUS"
                -> hesProgramacioMotiusDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesProgramacioMotiusDb2 {
                    tags "DatabaseRelation"
                }   
                -> mpi  {
                    tags "ExternalServiceRelation"
                }       
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }       
            }
            
            hesAlertesNotificacionsDb = softwaresystem "HES ALERTES I NOTIFICACIONS DB" "PostgreSQL" "Database,Postgres"
            hesAlertesNotificacions = softwaresystem "HES ALERTES I NOTIFICACIONS" "Alertes i Notificacions" "Information system"{
                 tags "APP:ALERTES I NOTIFICACIONS"
                 -> hesAlertesNotificacionsDb {
                    tags "DatabaseRelation"
                }   
                -> mpi {
                    tags "ExternalServiceRelation"
                }       
                -> pds {
                    tags "PdsRelation"
                }       
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }       
                -> ecap {
                    tags "ExternalServiceRelation"
                }       
            }
            
            hesPlanificatAdministratiuDb = softwaresystem "HES PLANIFICAT ADMINISTRATIU DB" "PostgreSQL" "Database,Postgres"
            hesPlanificatAdministratiu = softwaresystem "HES PLANIFICAT ADMINISTRATIU" "Planificat Administratiu" "Information system"{
                tags "APP:PLANIFICAT ADMINISTRATIU"
                -> hesPlanificatAdministratiuDb {
                    tags "DatabaseRelation"
                }   
                -> mpi {
                    tags "ExternalServiceRelation"
                }      
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "EventHubRelation"
                }
                -> hc3 {
                    tags "ExternalServiceRelation"
                }      
                -> ecap {
                    tags "ExternalServiceRelation"
                }      
            }
            
            hesAlergiesDb1 = softwaresystem "HES ALÈRGIES I REACCIONS ADVERSES DB1" "PostgreSQL" "Database,Postgres"
            hesAlergiesDb2 = softwaresystem "HES ALÈRGIES I REACCIONS ADVERSES DB2" "Redis" "Database,Redis"
            hesAlergies = softwaresystem "HES ALÈRGIES I REACCIONS ADVERSES" "Alèrgies i Reaccions Adverses" "Information system"{
                tags "APP:LÈRGIES I REACCIONS ADVERSES"
                -> hesAlergiesDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesAlergiesDb2 {
                    tags "DatabaseRelation"
                }   
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }      
                -> mpi {
                    tags "ExternalServiceRelation"
                }      
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                }  
                -> hc3 {
                    tags "ExternalServiceRelation"
                }      
                -> ecap {
                    tags "ExternalServiceRelation"
                }
                -> eventhub {
                    tags "EventHubRelation"
                }
            }
            
            hesGodDb1 = softwaresystem "HES GOD DB1" "PostgreSQL" "Database,Postgres"
            hesGodDb2 = softwaresystem "HES GOD DB2" "MongoDb" "Database,MongoDb"
            hesGod = softwaresystem "HES GOD" "Gestió de l’Oferta i la Demanda" "Information system"{
                tags "APP:GESTIÓ DE L’OFERTA I LA DEMANDA"
                -> hesGodDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesGodDb2 {
                    tags "DatabaseRelation"
                }   
                -> mpi {
                    tags "ExternalServiceRelation"
                }      
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                }      
                -> ecap {
                    tags "ExternalServiceRelation"
                }      
            }
            
            hesCipdsDb1 = softwaresystem "HES CIPDS DB1" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesCipdsDb2 = softwaresystem "HES CIPDS DB2" "Amazon Elastic Cache" "Database, AmazonElasticCache"
            hesCipds = softwaresystem "HES CIPDS" "Condicionants i Problemes de Salut" "Information system"{
                tags "APP:CONDICIONANTS I PROBLEMES DE SALUT"
                -> hesCipdsDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesCipdsDb2 {
                    tags "DatabaseRelation"
                }   
                -> mpi {
                    tags "ExternalServiceRelation"
                }      
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                }
                -> ecap {
                    tags "ExternalServiceRelation"
                }
            }
            
            hesTSTServidorTerminologicDb1 = softwaresystem "HES TST Servidor Terminològic DB1" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesTSTServidorTerminologicDb2 = softwaresystem "HES TST Servidor Terminològic DB2" "AWS ElasticCache" "Database,AmazonElasticCache"
            hesTSTServidorTerminologic = softwaresystem "HES TST Servidor Terminològic" "Servidor Terminològic al Cloud AWS" "Information system"{
                tags "APP:NOU SERVIDOR TERMINOLÒGIC"
                -> hesTSTServidorTerminologicDb1 {
                    tags "DatabaseRelation"
                }   
                -> hesTSTServidorTerminologicDb2 {
                    tags "DatabaseRelation"
                }   
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                }      
                -> ecap {
                    tags "ExternalServiceRelation"
                }      
            }
            hesDispensacionsDb = softwaresystem "HES DISP DB" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesDispensacions = softwaresystem "HES DISP" "Dispensacions" "Information system"{
                tags "APP:DISPENSACIONS"
                -> hesDispensacionsDb {
                    tags "DatabaseRelation"
                }   
               -> mpi {
                    tags "ExternalServiceRelation"
                }   
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                } 
                 -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                }  
               -> sire {
                    tags "ExternalServiceRelation"
                }    
                -> ecap {
                    tags "ExternalServiceRelation"
                } 
                -> formularisHes {
                    tags "ExternalServiceRelation"
                }  
                
            }
            
            hesHistorialSalutCatalunya = softwaresystem "HES HISTORIAL SALUT CATALUNYA" "Historial Salut Catalunya" "Information system"{
                 tags "APP:HISTORIAL SALUT CATALUNYA"
                 -> cdrRdc {
                    tags "ExternalServiceRelation"
                }      
               -> mpi {
                    tags "ExternalServiceRelation"
                }   
                -> pds {
                    tags "PdsRelation"
                }   
                -> plataformaDeAuditoria {
                    tags "PlataformaDeAuditoriaRelation"
                } 
                -> eventhub {
                    tags "ExternalServiceRelation"
                } 
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }   
            }
            
            hesFarmaDb1 = softwaresystem "HES FARMA (TFAR) DB1" "PostgreSQL" "Database,Postgres"
            hesFarmaDb2 = softwaresystem "HES FARMA (TFAR) DB2" "AmazonOpenSearch" "Database,AmazonOpenSearch"
            hesFarma = softwaresystem "HES FARMA (TFAR)" "Farma" "Information system"{
                tags "APP:FARMA (TFAR)"
                -> hesFarmaDb1 {
                    tags "DatabaseRelation"
                } 
                 -> hesFarmaDb2 {
                    tags "DatabaseRelation"
                }  
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }  
               -> mpi {
                    tags "ExternalServiceRelation"
                } 
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                } 
                -> sire {
                    tags "ExternalServiceRelation"
                }    
            }
            
            hesEda = softwaresystem "HES EDA" "eda" "Information system"{
               tags "APP:EDA"
               -> mpi {
                    tags "ExternalServiceRelation"
                } 
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                } 
            }
            hesConnectaDb1 = softwaresystem "HES CONNECTA DB1" "PostgreSQL" "Database,Postgres"
            hesConnectaDb2 = softwaresystem "HES CONNECTA DB2" "MongoDB Atlas" "Database,MongoDbAtlas"
            hesConnecta = softwaresystem "HES CONNECTA" "CONNECTA" "Information system"{
               tags "APP:CONNECTA"
               -> hesConnectaDb1 {
                    tags "DatabaseRelation"
                } 
                 -> hesConnectaDb2 {
                    tags "DatabaseRelation"
                }  
                -> cdrRdc {
                    tags "ExternalServiceRelation"
                }  
               -> mpi {
                    tags "ExternalServiceRelation"
                } 
                -> servidorTerminologic {
                    tags "ExternalServiceRelation"
                }
                -> pds {
                    tags "PdsRelation"
                }      
                -> eventhub {
                    tags "ExternalServiceRelation"
                } 
                 -> ecap {
                    tags "ExternalServiceRelation"
                }
                 -> formularisHes {
                    tags "ExternalServiceRelation"
                } 
                 -> lms {
                    tags "ExternalServiceRelation"
                } 
            }
            
            
        }
    
    }

    views {
        systemLandscape   "Diagram1" {
            include *
            autoLayout tb
        }
        styles {
            relationship "DatabaseRelation" {
                color #008000 
                thickness 2
                dashed false
                fontSize 18
            }
        
            relationship "ExternalServiceRelation" {
                color #ff5733 
                thickness 3
                dashed true
                fontSize 18
            }
            
            relationship "InternalServiceRelation" {
                color #800080
                thickness 2
                dashed false
                fontSize 18
            }
        
            relationship "EventHubRelation" {
                color #007bff
                thickness 2
                dashed true
                fontSize 18
            }
            
            relationship "PdsRelation" {
                color #FFCC00 
                thickness 3  
                dashed false 
                fontSize 18
            }

            relationship "PlataformaDeAuditoriaRelation" {
                color #00CED1
                thickness 2  
                dashed true 
                fontSize 18
            }

            
            element "Information system" {
                color #ffffff
                background #0D5091
            }
            
            element "Application" {
                color #ffffff
                background #438dd5
            }
            
            element "Database" {
                shape Cylinder
                color #ffffff
                
            }
            
            element "Oracle" {
                background #F80000
            }
            
            element "MongoDb" {
                background #47A248 
            }
            
            element "MongoDbAtlas" {
                background #47A248 
            }
            
            element "Postgres" {
                background #336791 
            }
            
            element "SqlServer" {
                background #A91D22 
            }
            
            element "Redis" {
                background #D82C20 
            }
            
            element "ElasticSearch" {
                background #005571
            }
            
            element "AmazonOpenSearch" {
                background #005571
            }
            
            element "AmazonElasticCache" {
                background #005571
            }
            
            element "MySql" {
                background #00758F 
            }

            
            element "frontend" {
                shape WebBrowser
            }
            
        }
    }
    configuration {
        scope softwaresystem
    }

}
```
