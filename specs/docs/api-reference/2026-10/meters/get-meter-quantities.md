> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Meter Quantities

> Get quantities of a meter over a time period.

**Scopes**: `meters:read` `meters:write`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/meters/{id}/quantities
openapi: 3.1.0
info:
  title: Polar API
  summary: Polar HTTP and Webhooks API
  description: Read the docs at https://polar.sh/docs/api-reference
  version: 2026-10
servers:
  - url: https://api.polar.sh
    description: Production environment
    x-speakeasy-server-id: production
    x-polar-environment: production
  - url: https://sandbox-api.polar.sh
    description: Sandbox environment
    x-speakeasy-server-id: sandbox
    x-polar-environment: sandbox
security: []
tags:
  - name: public
    description: >-
      Endpoints shown and documented in the Polar API documentation and
      available in our SDKs.
  - name: private
    description: >-
      Endpoints that should appear in the schema only in development to generate
      our internal JS SDK.
paths:
  /v1/meters/{id}/quantities:
    get:
      tags:
        - meters
        - public
      summary: Get Meter Quantities
      description: |-
        Get quantities of a meter over a time period.

        **Scopes**: `meters:read` `meters:write`
      operationId: meters:quantities
      parameters:
        - name: id
          in: path
          required: true
          schema:
            type: string
            format: uuid4
            description: The meter ID.
            title: Id
          description: The meter ID.
        - name: start_timestamp
          in: query
          required: true
          schema:
            type: string
            format: date-time
            description: Start timestamp.
            title: Start Timestamp
          description: Start timestamp.
        - name: end_timestamp
          in: query
          required: true
          schema:
            type: string
            format: date-time
            description: End timestamp.
            title: End Timestamp
          description: End timestamp.
        - name: interval
          in: query
          required: true
          schema:
            $ref: '#/components/schemas/TimeInterval'
            description: Interval between two timestamps.
          description: Interval between two timestamps.
        - name: timezone
          in: query
          required: false
          schema:
            type: string
            minLength: 1
            description: Timezone to use for the timestamps. Default is UTC.
            enum:
              - Africa/Abidjan
              - Africa/Accra
              - Africa/Addis_Ababa
              - Africa/Algiers
              - Africa/Asmara
              - Africa/Asmera
              - Africa/Bamako
              - Africa/Bangui
              - Africa/Banjul
              - Africa/Bissau
              - Africa/Blantyre
              - Africa/Brazzaville
              - Africa/Bujumbura
              - Africa/Cairo
              - Africa/Casablanca
              - Africa/Ceuta
              - Africa/Conakry
              - Africa/Dakar
              - Africa/Dar_es_Salaam
              - Africa/Djibouti
              - Africa/Douala
              - Africa/El_Aaiun
              - Africa/Freetown
              - Africa/Gaborone
              - Africa/Harare
              - Africa/Johannesburg
              - Africa/Juba
              - Africa/Kampala
              - Africa/Khartoum
              - Africa/Kigali
              - Africa/Kinshasa
              - Africa/Lagos
              - Africa/Libreville
              - Africa/Lome
              - Africa/Luanda
              - Africa/Lubumbashi
              - Africa/Lusaka
              - Africa/Malabo
              - Africa/Maputo
              - Africa/Maseru
              - Africa/Mbabane
              - Africa/Mogadishu
              - Africa/Monrovia
              - Africa/Nairobi
              - Africa/Ndjamena
              - Africa/Niamey
              - Africa/Nouakchott
              - Africa/Ouagadougou
              - Africa/Porto-Novo
              - Africa/Sao_Tome
              - Africa/Timbuktu
              - Africa/Tripoli
              - Africa/Tunis
              - Africa/Windhoek
              - America/Adak
              - America/Anchorage
              - America/Anguilla
              - America/Antigua
              - America/Araguaina
              - America/Argentina/Buenos_Aires
              - America/Argentina/Catamarca
              - America/Argentina/ComodRivadavia
              - America/Argentina/Cordoba
              - America/Argentina/Jujuy
              - America/Argentina/La_Rioja
              - America/Argentina/Mendoza
              - America/Argentina/Rio_Gallegos
              - America/Argentina/Salta
              - America/Argentina/San_Juan
              - America/Argentina/San_Luis
              - America/Argentina/Tucuman
              - America/Argentina/Ushuaia
              - America/Aruba
              - America/Asuncion
              - America/Atikokan
              - America/Atka
              - America/Bahia
              - America/Bahia_Banderas
              - America/Barbados
              - America/Belem
              - America/Belize
              - America/Blanc-Sablon
              - America/Boa_Vista
              - America/Bogota
              - America/Boise
              - America/Buenos_Aires
              - America/Cambridge_Bay
              - America/Campo_Grande
              - America/Cancun
              - America/Caracas
              - America/Catamarca
              - America/Cayenne
              - America/Cayman
              - America/Chicago
              - America/Chihuahua
              - America/Ciudad_Juarez
              - America/Coral_Harbour
              - America/Cordoba
              - America/Costa_Rica
              - America/Coyhaique
              - America/Creston
              - America/Cuiaba
              - America/Curacao
              - America/Danmarkshavn
              - America/Dawson
              - America/Dawson_Creek
              - America/Denver
              - America/Detroit
              - America/Dominica
              - America/Edmonton
              - America/Eirunepe
              - America/El_Salvador
              - America/Ensenada
              - America/Fort_Nelson
              - America/Fort_Wayne
              - America/Fortaleza
              - America/Glace_Bay
              - America/Godthab
              - America/Goose_Bay
              - America/Grand_Turk
              - America/Grenada
              - America/Guadeloupe
              - America/Guatemala
              - America/Guayaquil
              - America/Guyana
              - America/Halifax
              - America/Havana
              - America/Hermosillo
              - America/Indiana/Indianapolis
              - America/Indiana/Knox
              - America/Indiana/Marengo
              - America/Indiana/Petersburg
              - America/Indiana/Tell_City
              - America/Indiana/Vevay
              - America/Indiana/Vincennes
              - America/Indiana/Winamac
              - America/Indianapolis
              - America/Inuvik
              - America/Iqaluit
              - America/Jamaica
              - America/Jujuy
              - America/Juneau
              - America/Kentucky/Louisville
              - America/Kentucky/Monticello
              - America/Knox_IN
              - America/Kralendijk
              - America/La_Paz
              - America/Lima
              - America/Los_Angeles
              - America/Louisville
              - America/Lower_Princes
              - America/Maceio
              - America/Managua
              - America/Manaus
              - America/Marigot
              - America/Martinique
              - America/Matamoros
              - America/Mazatlan
              - America/Mendoza
              - America/Menominee
              - America/Merida
              - America/Metlakatla
              - America/Mexico_City
              - America/Miquelon
              - America/Moncton
              - America/Monterrey
              - America/Montevideo
              - America/Montreal
              - America/Montserrat
              - America/Nassau
              - America/New_York
              - America/Nipigon
              - America/Nome
              - America/Noronha
              - America/North_Dakota/Beulah
              - America/North_Dakota/Center
              - America/North_Dakota/New_Salem
              - America/Nuuk
              - America/Ojinaga
              - America/Panama
              - America/Pangnirtung
              - America/Paramaribo
              - America/Phoenix
              - America/Port-au-Prince
              - America/Port_of_Spain
              - America/Porto_Acre
              - America/Porto_Velho
              - America/Puerto_Rico
              - America/Punta_Arenas
              - America/Rainy_River
              - America/Rankin_Inlet
              - America/Recife
              - America/Regina
              - America/Resolute
              - America/Rio_Branco
              - America/Rosario
              - America/Santa_Isabel
              - America/Santarem
              - America/Santiago
              - America/Santo_Domingo
              - America/Sao_Paulo
              - America/Scoresbysund
              - America/Shiprock
              - America/Sitka
              - America/St_Barthelemy
              - America/St_Johns
              - America/St_Kitts
              - America/St_Lucia
              - America/St_Thomas
              - America/St_Vincent
              - America/Swift_Current
              - America/Tegucigalpa
              - America/Thule
              - America/Thunder_Bay
              - America/Tijuana
              - America/Toronto
              - America/Tortola
              - America/Vancouver
              - America/Virgin
              - America/Whitehorse
              - America/Winnipeg
              - America/Yakutat
              - America/Yellowknife
              - Antarctica/Casey
              - Antarctica/Davis
              - Antarctica/DumontDUrville
              - Antarctica/Macquarie
              - Antarctica/Mawson
              - Antarctica/McMurdo
              - Antarctica/Palmer
              - Antarctica/Rothera
              - Antarctica/South_Pole
              - Antarctica/Syowa
              - Antarctica/Troll
              - Antarctica/Vostok
              - Arctic/Longyearbyen
              - Asia/Aden
              - Asia/Almaty
              - Asia/Amman
              - Asia/Anadyr
              - Asia/Aqtau
              - Asia/Aqtobe
              - Asia/Ashgabat
              - Asia/Ashkhabad
              - Asia/Atyrau
              - Asia/Baghdad
              - Asia/Bahrain
              - Asia/Baku
              - Asia/Bangkok
              - Asia/Barnaul
              - Asia/Beirut
              - Asia/Bishkek
              - Asia/Brunei
              - Asia/Calcutta
              - Asia/Chita
              - Asia/Choibalsan
              - Asia/Chongqing
              - Asia/Chungking
              - Asia/Colombo
              - Asia/Dacca
              - Asia/Damascus
              - Asia/Dhaka
              - Asia/Dili
              - Asia/Dubai
              - Asia/Dushanbe
              - Asia/Famagusta
              - Asia/Gaza
              - Asia/Harbin
              - Asia/Hebron
              - Asia/Ho_Chi_Minh
              - Asia/Hong_Kong
              - Asia/Hovd
              - Asia/Irkutsk
              - Asia/Istanbul
              - Asia/Jakarta
              - Asia/Jayapura
              - Asia/Jerusalem
              - Asia/Kabul
              - Asia/Kamchatka
              - Asia/Karachi
              - Asia/Kashgar
              - Asia/Kathmandu
              - Asia/Katmandu
              - Asia/Khandyga
              - Asia/Kolkata
              - Asia/Krasnoyarsk
              - Asia/Kuala_Lumpur
              - Asia/Kuching
              - Asia/Kuwait
              - Asia/Macao
              - Asia/Macau
              - Asia/Magadan
              - Asia/Makassar
              - Asia/Manila
              - Asia/Muscat
              - Asia/Nicosia
              - Asia/Novokuznetsk
              - Asia/Novosibirsk
              - Asia/Omsk
              - Asia/Oral
              - Asia/Phnom_Penh
              - Asia/Pontianak
              - Asia/Pyongyang
              - Asia/Qatar
              - Asia/Qostanay
              - Asia/Qyzylorda
              - Asia/Rangoon
              - Asia/Riyadh
              - Asia/Saigon
              - Asia/Sakhalin
              - Asia/Samarkand
              - Asia/Seoul
              - Asia/Shanghai
              - Asia/Singapore
              - Asia/Srednekolymsk
              - Asia/Taipei
              - Asia/Tashkent
              - Asia/Tbilisi
              - Asia/Tehran
              - Asia/Tel_Aviv
              - Asia/Thimbu
              - Asia/Thimphu
              - Asia/Tokyo
              - Asia/Tomsk
              - Asia/Ujung_Pandang
              - Asia/Ulaanbaatar
              - Asia/Ulan_Bator
              - Asia/Urumqi
              - Asia/Ust-Nera
              - Asia/Vientiane
              - Asia/Vladivostok
              - Asia/Yakutsk
              - Asia/Yangon
              - Asia/Yekaterinburg
              - Asia/Yerevan
              - Atlantic/Azores
              - Atlantic/Bermuda
              - Atlantic/Canary
              - Atlantic/Cape_Verde
              - Atlantic/Faeroe
              - Atlantic/Faroe
              - Atlantic/Jan_Mayen
              - Atlantic/Madeira
              - Atlantic/Reykjavik
              - Atlantic/South_Georgia
              - Atlantic/St_Helena
              - Atlantic/Stanley
              - Australia/ACT
              - Australia/Adelaide
              - Australia/Brisbane
              - Australia/Broken_Hill
              - Australia/Canberra
              - Australia/Currie
              - Australia/Darwin
              - Australia/Eucla
              - Australia/Hobart
              - Australia/LHI
              - Australia/Lindeman
              - Australia/Lord_Howe
              - Australia/Melbourne
              - Australia/NSW
              - Australia/North
              - Australia/Perth
              - Australia/Queensland
              - Australia/South
              - Australia/Sydney
              - Australia/Tasmania
              - Australia/Victoria
              - Australia/West
              - Australia/Yancowinna
              - Brazil/Acre
              - Brazil/DeNoronha
              - Brazil/East
              - Brazil/West
              - CET
              - CST6CDT
              - Canada/Atlantic
              - Canada/Central
              - Canada/Eastern
              - Canada/Mountain
              - Canada/Newfoundland
              - Canada/Pacific
              - Canada/Saskatchewan
              - Canada/Yukon
              - Chile/Continental
              - Chile/EasterIsland
              - Cuba
              - EET
              - EST
              - EST5EDT
              - Egypt
              - Eire
              - Etc/GMT
              - Etc/GMT+0
              - Etc/GMT+1
              - Etc/GMT+10
              - Etc/GMT+11
              - Etc/GMT+12
              - Etc/GMT+2
              - Etc/GMT+3
              - Etc/GMT+4
              - Etc/GMT+5
              - Etc/GMT+6
              - Etc/GMT+7
              - Etc/GMT+8
              - Etc/GMT+9
              - Etc/GMT-0
              - Etc/GMT-1
              - Etc/GMT-10
              - Etc/GMT-11
              - Etc/GMT-12
              - Etc/GMT-13
              - Etc/GMT-14
              - Etc/GMT-2
              - Etc/GMT-3
              - Etc/GMT-4
              - Etc/GMT-5
              - Etc/GMT-6
              - Etc/GMT-7
              - Etc/GMT-8
              - Etc/GMT-9
              - Etc/GMT0
              - Etc/Greenwich
              - Etc/UCT
              - Etc/UTC
              - Etc/Universal
              - Etc/Zulu
              - Europe/Amsterdam
              - Europe/Andorra
              - Europe/Astrakhan
              - Europe/Athens
              - Europe/Belfast
              - Europe/Belgrade
              - Europe/Berlin
              - Europe/Bratislava
              - Europe/Brussels
              - Europe/Bucharest
              - Europe/Budapest
              - Europe/Busingen
              - Europe/Chisinau
              - Europe/Copenhagen
              - Europe/Dublin
              - Europe/Gibraltar
              - Europe/Guernsey
              - Europe/Helsinki
              - Europe/Isle_of_Man
              - Europe/Istanbul
              - Europe/Jersey
              - Europe/Kaliningrad
              - Europe/Kiev
              - Europe/Kirov
              - Europe/Kyiv
              - Europe/Lisbon
              - Europe/Ljubljana
              - Europe/London
              - Europe/Luxembourg
              - Europe/Madrid
              - Europe/Malta
              - Europe/Mariehamn
              - Europe/Minsk
              - Europe/Monaco
              - Europe/Moscow
              - Europe/Nicosia
              - Europe/Oslo
              - Europe/Paris
              - Europe/Podgorica
              - Europe/Prague
              - Europe/Riga
              - Europe/Rome
              - Europe/Samara
              - Europe/San_Marino
              - Europe/Sarajevo
              - Europe/Saratov
              - Europe/Simferopol
              - Europe/Skopje
              - Europe/Sofia
              - Europe/Stockholm
              - Europe/Tallinn
              - Europe/Tirane
              - Europe/Tiraspol
              - Europe/Ulyanovsk
              - Europe/Uzhgorod
              - Europe/Vaduz
              - Europe/Vatican
              - Europe/Vienna
              - Europe/Vilnius
              - Europe/Volgograd
              - Europe/Warsaw
              - Europe/Zagreb
              - Europe/Zaporozhye
              - Europe/Zurich
              - Factory
              - GB
              - GB-Eire
              - GMT
              - GMT+0
              - GMT-0
              - GMT0
              - Greenwich
              - HST
              - Hongkong
              - Iceland
              - Indian/Antananarivo
              - Indian/Chagos
              - Indian/Christmas
              - Indian/Cocos
              - Indian/Comoro
              - Indian/Kerguelen
              - Indian/Mahe
              - Indian/Maldives
              - Indian/Mauritius
              - Indian/Mayotte
              - Indian/Reunion
              - Iran
              - Israel
              - Jamaica
              - Japan
              - Kwajalein
              - Libya
              - MET
              - MST
              - MST7MDT
              - Mexico/BajaNorte
              - Mexico/BajaSur
              - Mexico/General
              - NZ
              - NZ-CHAT
              - Navajo
              - PRC
              - PST8PDT
              - Pacific/Apia
              - Pacific/Auckland
              - Pacific/Bougainville
              - Pacific/Chatham
              - Pacific/Chuuk
              - Pacific/Easter
              - Pacific/Efate
              - Pacific/Enderbury
              - Pacific/Fakaofo
              - Pacific/Fiji
              - Pacific/Funafuti
              - Pacific/Galapagos
              - Pacific/Gambier
              - Pacific/Guadalcanal
              - Pacific/Guam
              - Pacific/Honolulu
              - Pacific/Johnston
              - Pacific/Kanton
              - Pacific/Kiritimati
              - Pacific/Kosrae
              - Pacific/Kwajalein
              - Pacific/Majuro
              - Pacific/Marquesas
              - Pacific/Midway
              - Pacific/Nauru
              - Pacific/Niue
              - Pacific/Norfolk
              - Pacific/Noumea
              - Pacific/Pago_Pago
              - Pacific/Palau
              - Pacific/Pitcairn
              - Pacific/Pohnpei
              - Pacific/Ponape
              - Pacific/Port_Moresby
              - Pacific/Rarotonga
              - Pacific/Saipan
              - Pacific/Samoa
              - Pacific/Tahiti
              - Pacific/Tarawa
              - Pacific/Tongatapu
              - Pacific/Truk
              - Pacific/Wake
              - Pacific/Wallis
              - Pacific/Yap
              - Poland
              - Portugal
              - ROC
              - ROK
              - Singapore
              - Turkey
              - UCT
              - US/Alaska
              - US/Aleutian
              - US/Arizona
              - US/Central
              - US/East-Indiana
              - US/Eastern
              - US/Hawaii
              - US/Indiana-Starke
              - US/Michigan
              - US/Mountain
              - US/Pacific
              - US/Samoa
              - UTC
              - Universal
              - W-SU
              - WET
              - Zulu
            default: UTC
            title: Timezone
          description: Timezone to use for the timestamps. Default is UTC.
        - name: customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The customer ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The customer ID.
              - type: 'null'
            title: CustomerID Filter
            description: Filter by customer ID.
          description: Filter by customer ID.
        - name: external_customer_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
              - items:
                  type: string
                type: array
              - type: 'null'
            title: ExternalCustomerID Filter
            description: Filter by external customer ID.
          description: Filter by external customer ID.
        - name: customer_aggregation_function
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/AggregationFunction'
              - type: 'null'
            description: >-
              If set, will first compute the quantities per customer before
              aggregating them using the given function. If not set, the
              quantities will be aggregated across all events.
            title: Customer Aggregation Function
          description: >-
            If set, will first compute the quantities per customer before
            aggregating them using the given function. If not set, the
            quantities will be aggregated across all events.
        - name: metadata
          in: query
          required: false
          style: deepObject
          schema:
            $ref: '#/components/schemas/MetadataQuery'
          description: >-
            Filter by metadata key-value pairs. It uses the `deepObject` style,
            e.g. `?metadata[key]=value`.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MeterQuantities'
        '404':
          description: Meter not found.
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/ResourceNotFound'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - meters:read
            - meters:write
        - pat:
            - meters:read
            - meters:write
        - oat:
            - meters:read
            - meters:write
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.meters.quantities(
                '00000000-0000-4000-8000-000000000000',
                start_timestamp='2026-01-01T00:00:00Z',
                end_timestamp='2026-01-01T00:00:00Z',
                interval='year',
                timezone='UTC',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.meters.quantities(
              "00000000-0000-4000-8000-000000000000",
              {
                "start_timestamp": "2026-01-01T00:00:00Z",
                "end_timestamp": "2026-01-01T00:00:00Z",
                "interval": "year",
                "timezone": "UTC"
              },
            );
            console.log(response);
components:
  schemas:
    TimeInterval:
      type: string
      enum:
        - year
        - month
        - week
        - day
        - hour
      title: TimeInterval
    AggregationFunction:
      type: string
      enum:
        - count
        - sum
        - max
        - min
        - avg
        - unique
      title: AggregationFunction
    MetadataQuery:
      anyOf:
        - type: object
          additionalProperties:
            anyOf:
              - type: string
              - type: integer
              - type: boolean
              - items:
                  type: string
                type: array
              - type: array
                items:
                  type: integer
              - type: array
                items:
                  type: boolean
        - type: 'null'
      title: MetadataQuery
    MeterQuantities:
      properties:
        quantities:
          items:
            $ref: '#/components/schemas/MeterQuantity'
          type: array
          title: Quantities
        total:
          type: number
          title: Total
          description: The total quantity for the period.
          examples:
            - 100
      type: object
      required:
        - quantities
        - total
      title: MeterQuantities
    ResourceNotFound:
      properties:
        error:
          type: string
          const: ResourceNotFound
          title: Error
          examples:
            - ResourceNotFound
        detail:
          type: string
          title: Detail
      type: object
      required:
        - error
        - detail
      title: ResourceNotFound
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    MeterQuantity:
      properties:
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: The timestamp for the current period.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        quantity:
          type: number
          title: Quantity
          description: The quantity for the current period.
          examples:
            - 10
      type: object
      required:
        - timestamp
        - quantity
      title: MeterQuantity
    ValidationError:
      properties:
        loc:
          items:
            anyOf:
              - type: string
              - type: integer
          type: array
          title: Location
        msg:
          type: string
          title: Message
        type:
          type: string
          title: Error Type
        input:
          title: Input
        ctx:
          type: object
          title: Context
      type: object
      required:
        - loc
        - msg
        - type
      title: ValidationError
  securitySchemes:
    oidc:
      type: openIdConnect
      openIdConnectUrl: /.well-known/openid-configuration
    pat:
      type: http
      description: >-
        You can generate a **Personal Access Token** from your
        [settings](https://polar.sh/settings).
      scheme: bearer
    oat:
      type: http
      description: >-
        You can generate an **Organization Access Token** from your
        organization's settings.
      scheme: bearer

````
