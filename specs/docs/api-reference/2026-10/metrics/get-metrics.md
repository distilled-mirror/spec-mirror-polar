> ## Documentation Index
> Fetch the complete documentation index at: https://polar.sh/docs/llms.txt
> Use this file to discover all available pages before exploring further.

# Get Metrics

> Get metrics about your orders and subscriptions.

Currency values are output in cents.

**Scopes**: `metrics:read`



## OpenAPI

````yaml /openapi/2026-10.openapi.json get /v1/metrics/
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
  /v1/metrics/:
    get:
      tags:
        - metrics
        - public
      summary: Get Metrics
      description: |-
        Get metrics about your orders and subscriptions.

        Currency values are output in cents.

        **Scopes**: `metrics:read`
      operationId: metrics:get
      parameters:
        - name: start_date
          in: query
          required: true
          schema:
            type: string
            format: date
            description: Start date.
            title: Start Date
          description: Start date.
        - name: end_date
          in: query
          required: true
          schema:
            type: string
            format: date
            description: End date.
            title: End Date
          description: End date.
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
        - name: interval
          in: query
          required: true
          schema:
            $ref: '#/components/schemas/TimeInterval'
            description: Interval between two timestamps.
          description: Interval between two timestamps.
        - name: organization_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The organization ID.
                examples:
                  - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The organization ID.
                  examples:
                    - 1dbfc517-0bbf-4301-9ba8-555ca42b9737
              - type: 'null'
            title: OrganizationID Filter
            description: Filter by organization ID.
          description: Filter by organization ID.
        - name: product_id
          in: query
          required: false
          schema:
            anyOf:
              - type: string
                format: uuid4
                description: The product ID.
              - type: array
                items:
                  type: string
                  format: uuid4
                  description: The product ID.
              - type: 'null'
            title: ProductID Filter
            description: Filter by product ID.
          description: Filter by product ID.
        - name: billing_type
          in: query
          required: false
          schema:
            anyOf:
              - $ref: '#/components/schemas/ProductBillingType'
              - type: array
                items:
                  $ref: '#/components/schemas/ProductBillingType'
              - type: 'null'
            title: ProductBillingType Filter
            description: >-
              Filter by billing type. `recurring` will filter data corresponding
              to subscriptions creations or renewals. `one_time` will filter
              data corresponding to one-time purchases.
          description: >-
            Filter by billing type. `recurring` will filter data corresponding
            to subscriptions creations or renewals. `one_time` will filter data
            corresponding to one-time purchases.
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
        - name: metrics
          in: query
          required: false
          schema:
            anyOf:
              - items:
                  type: string
                type: array
              - type: 'null'
            title: Metrics
            description: >-
              List of metric slugs to focus on. When provided, only the queries
              needed for these metrics will be executed, improving performance.
              If not provided, all metrics are returned.
          description: >-
            List of metric slugs to focus on. When provided, only the queries
            needed for these metrics will be executed, improving performance. If
            not provided, all metrics are returned.
      responses:
        '200':
          description: Successful Response
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/MetricsResponse'
        '422':
          description: Validation Error
          content:
            application/json:
              schema:
                $ref: '#/components/schemas/HTTPValidationError'
      security:
        - oidc:
            - metrics:read
        - pat:
            - metrics:read
        - oat:
            - metrics:read
      x-codeSamples:
        - lang: python
          source: |
            from polar.v2026_10 import Polar

            polar = Polar("polar_oat_xxx")

            response = polar.metrics.get(
                start_date='2026-01-01',
                end_date='2026-01-01',
                timezone='UTC',
                interval='year',
            )
            print(response)
        - lang: typescript
          source: |
            import { createPolar } from "@polar-sh/sdk/2026-10";

            const polar = createPolar({
              accessToken: "polar_oat_xxx",
            });

            const response = await polar.metrics.get(
              {
                "start_date": "2026-01-01",
                "end_date": "2026-01-01",
                "timezone": "UTC",
                "interval": "year"
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
    ProductBillingType:
      type: string
      enum:
        - one_time
        - recurring
      title: ProductBillingType
    MetricsResponse:
      properties:
        periods:
          items:
            $ref: '#/components/schemas/MetricPeriod'
          type: array
          title: Periods
          description: List of data for each timestamp.
        totals:
          $ref: '#/components/schemas/MetricsTotals'
          description: Totals for the whole selected period.
        metrics:
          $ref: '#/components/schemas/Metrics'
          description: Information about the returned metrics.
      type: object
      required:
        - periods
        - totals
        - metrics
      title: MetricsResponse
      description: Metrics response schema.
    HTTPValidationError:
      properties:
        detail:
          items:
            $ref: '#/components/schemas/ValidationError'
          type: array
          title: Detail
      type: object
      title: HTTPValidationError
    MetricPeriod:
      properties:
        timestamp:
          type: string
          format: date-time
          title: Timestamp
          description: Timestamp of this period data.
          examples:
            - '2026-01-01T00:00:00.000000Z'
        active_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Active Subscriptions
        committed_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Subscriptions
        monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Monthly Recurring Revenue
        trial_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Trial Monthly Recurring Revenue
        committed_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Monthly Recurring Revenue
        trial_committed_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Trial Committed Monthly Recurring Revenue
        average_revenue_per_user:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Revenue Per User
        checkouts:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Checkouts
        succeeded_checkouts:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Succeeded Checkouts
        churned_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churned Subscriptions
        churn_rate:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churn Rate
        seats_total:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Total
        seats_claimed:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Claimed
        seats_pending:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Pending
        seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seat Customers
        new_seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Seat Customers
        churned_seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churned Seat Customers
        orders:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Orders
        revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Revenue
        net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Revenue
        cumulative_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cumulative Revenue
        net_cumulative_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Cumulative Revenue
        costs:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Costs
        cumulative_costs:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cumulative Costs
        average_order_value:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Order Value
        net_average_order_value:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Average Order Value
        cost_per_user:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cost Per User
        active_user_by_event:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Active User By Event
        one_time_products:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products
        one_time_products_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products Revenue
        one_time_products_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products Net Revenue
        new_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions
        new_subscriptions_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions Revenue
        new_subscriptions_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions Net Revenue
        renewed_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions
        renewed_subscriptions_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions Revenue
        renewed_subscriptions_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions Net Revenue
        canceled_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions
        canceled_subscriptions_customer_service:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Customer Service
        canceled_subscriptions_low_quality:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Low Quality
        canceled_subscriptions_missing_features:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Missing Features
        canceled_subscriptions_switched_service:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Switched Service
        canceled_subscriptions_too_complex:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Too Complex
        canceled_subscriptions_too_expensive:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Too Expensive
        canceled_subscriptions_unused:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Unused
        canceled_subscriptions_other:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Other
        annual_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Annual Recurring Revenue
        committed_annual_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Annual Recurring Revenue
        checkouts_conversion:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Checkouts Conversion
        ltv:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Ltv
        gross_margin:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Gross Margin
        gross_margin_percentage:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Gross Margin Percentage
        cashflow:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cashflow
        average_seats_per_customer:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Seats Per Customer
        seat_utilization_rate:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seat Utilization Rate
      type: object
      required:
        - timestamp
      title: MetricPeriod
    MetricsTotals:
      properties:
        active_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Active Subscriptions
        committed_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Subscriptions
        monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Monthly Recurring Revenue
        trial_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Trial Monthly Recurring Revenue
        committed_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Monthly Recurring Revenue
        trial_committed_monthly_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Trial Committed Monthly Recurring Revenue
        average_revenue_per_user:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Revenue Per User
        checkouts:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Checkouts
        succeeded_checkouts:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Succeeded Checkouts
        churned_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churned Subscriptions
        churn_rate:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churn Rate
        seats_total:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Total
        seats_claimed:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Claimed
        seats_pending:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seats Pending
        seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seat Customers
        new_seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Seat Customers
        churned_seat_customers:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Churned Seat Customers
        orders:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Orders
        revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Revenue
        net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Revenue
        cumulative_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cumulative Revenue
        net_cumulative_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Cumulative Revenue
        costs:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Costs
        cumulative_costs:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cumulative Costs
        average_order_value:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Order Value
        net_average_order_value:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Net Average Order Value
        cost_per_user:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cost Per User
        active_user_by_event:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Active User By Event
        one_time_products:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products
        one_time_products_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products Revenue
        one_time_products_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: One Time Products Net Revenue
        new_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions
        new_subscriptions_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions Revenue
        new_subscriptions_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: New Subscriptions Net Revenue
        renewed_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions
        renewed_subscriptions_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions Revenue
        renewed_subscriptions_net_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Renewed Subscriptions Net Revenue
        canceled_subscriptions:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions
        canceled_subscriptions_customer_service:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Customer Service
        canceled_subscriptions_low_quality:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Low Quality
        canceled_subscriptions_missing_features:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Missing Features
        canceled_subscriptions_switched_service:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Switched Service
        canceled_subscriptions_too_complex:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Too Complex
        canceled_subscriptions_too_expensive:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Too Expensive
        canceled_subscriptions_unused:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Unused
        canceled_subscriptions_other:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Canceled Subscriptions Other
        annual_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Annual Recurring Revenue
        committed_annual_recurring_revenue:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Committed Annual Recurring Revenue
        checkouts_conversion:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Checkouts Conversion
        ltv:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Ltv
        gross_margin:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Gross Margin
        gross_margin_percentage:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Gross Margin Percentage
        cashflow:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Cashflow
        average_seats_per_customer:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Average Seats Per Customer
        seat_utilization_rate:
          anyOf:
            - type: integer
            - type: number
            - type: 'null'
          title: Seat Utilization Rate
      type: object
      title: MetricsTotals
    Metrics:
      properties:
        active_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        committed_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        monthly_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        trial_monthly_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        committed_monthly_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        trial_committed_monthly_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        average_revenue_per_user:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        checkouts:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        succeeded_checkouts:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        churned_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        churn_rate:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        seats_total:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        seats_claimed:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        seats_pending:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        seat_customers:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        new_seat_customers:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        churned_seat_customers:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        orders:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        net_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        cumulative_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        net_cumulative_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        costs:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        cumulative_costs:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        average_order_value:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        net_average_order_value:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        cost_per_user:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        active_user_by_event:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        one_time_products:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        one_time_products_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        one_time_products_net_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        new_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        new_subscriptions_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        new_subscriptions_net_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        renewed_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        renewed_subscriptions_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        renewed_subscriptions_net_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_customer_service:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_low_quality:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_missing_features:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_switched_service:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_too_complex:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_too_expensive:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_unused:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        canceled_subscriptions_other:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        annual_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        committed_annual_recurring_revenue:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        checkouts_conversion:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        ltv:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        gross_margin:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        gross_margin_percentage:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        cashflow:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        average_seats_per_customer:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
        seat_utilization_rate:
          anyOf:
            - $ref: '#/components/schemas/Metric'
            - type: 'null'
      type: object
      title: Metrics
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
    Metric:
      properties:
        slug:
          type: string
          title: Slug
          description: Unique identifier for the metric.
        display_name:
          type: string
          title: Display Name
          description: Human-readable name for the metric.
        type:
          $ref: '#/components/schemas/MetricType'
          description: Type of the metric, useful to know the unit or format of the value.
      type: object
      required:
        - slug
        - display_name
        - type
      title: Metric
      description: Information about a metric.
    MetricType:
      type: string
      enum:
        - scalar
        - currency
        - currency_sub_cent
        - percentage
      title: MetricType
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
