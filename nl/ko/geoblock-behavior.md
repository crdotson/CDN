---

copyright:
  years: 2018
lastupdated: "2018-07-25"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:codeblock: .codeblock}
{:pre: .pre}
{:screen: .screen}
{:tip: .tip}
{:download: .download}

# 지역 차단 클래스
`SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking` 클래스에는 지역 차단 API가 이용하는 속성이 포함되어 있습니다. 지역 차단 API의 각각은 이 유형의 오브젝트를 리턴하고 CDN에서 지리적 액세스 제어 동작을 설정하는 데 사용됩니다.

클래스 `SoftLayer_Network_CdnMarketplace_Configuration_Behavior_Geoblocking`:

* `accessType`: 규칙이 지정된 지역을 `ALLOW` 또는 `DENY`해야 하는지 지정합니다.
* `regionType`: 지역 차단 규칙을 적용할 지역의 유형(`CONTINENT` 또는 `COUNTRY_OR_REGION`)
* `regions`: `regionType`을 기반으로 하는 값 배열
* `status`: 현재 규칙의 상태(`ACTIVE` 또는 `INACTIVE`)

`regionType` = `CONTINENT`인 경우,
```
  regions = [
                  Africa,
                  Asia,
                  Europe,
                  North America,
                  Oceania,
                  Others,
                  South America
              ]
```

`regionType` = `COUNTRY_OR_REGION`인 경우,
```
  regions = [
                    Andorra,
                    United Arab Emirates,
                    Afghanistan,
                    Antigua and Barbuda,
                    Anguilla,
                    Albania,
                    Armenia,
                    Angola,
                    Antarctica,
                    Argentina,
                    American Samoa,
                    Austria,
                    Australia,
                    Aruba,
                    Azerbaijan,
                    Bosnia and Herzegovina,
                    Barbados,
                    Bangladesh,
                    Belgium,
                    Burkina Faso,
                    Bulgaria,
                    Bahrain,
                    Burundi,
                    Benin,
                    Saint Barthelemy,
                    Bermuda,
                    Brunei Darussalam,
                    Bolivia,
                    Bonaire, Saint Eustatius and Saba,
                    Brazil,
                    Bahamas,
                    Bhutan,
                    Bouvet Island,
                    Botswana,
                    Belarus,
                    Belize,
                    Canada,
                    Cocos (Keeling) Islands,
                    Congo, Democratic Republic of the,
                    Central African Republic,
                    Congo,
                    Switzerland,
                    Cote d\'Ivoire,
                    Cook Islands,
                    Chile,
                    Cameroon,
                    China,
                    Colombia,
                    Costa Rica,
                    Cuba,
                    Cape Verde,
                    Curacao,
                    Christmas Island,
                    Cyprus,
                    Czech Republic,
                    Germany,
                    Djibouti,
                    Denmark,
                    Dominica,
                    Dominican Republic,
                    Algeria,
                    Ecuador,
                    Estonia,
                    Egypt,
                    Western Sahara,
                    Eritrea,
                    Spain,
                    Ethiopia,
                    Europe (generic),
                    Finland,
                    Fiji,
                    Falkland Islands (Malvinas),
                    Micronesia, Federated States of,
                    Faroe Islands,
                    France,
                    Gabon,
                    United Kingdom,
                    Grenada,
                    Georgia,
                    French Guiana,
                    Ghana,
                    Gibraltar,
                    Guernsey,
                    Greenland,
                    Gambia,
                    Guinea,
                    Guadeloupe,
                    Equatorial Guinea,
                    Greece,
                    South Georgia/South Sandwich Isl.,
                    Guatemala,
                    Guam,
                    Guinea-Bissau,
                    Guyana,
                    Hong Kong S.A.R. of China (PRC),
                    Heard Island and Mcdonald Islands,
                    Honduras,
                    Croatia,
                    Haiti,
                    Hungary,
                    Indonesia,
                    Ireland,
                    Israel,
                    Isle of Man,
                    India,
                    British Indian Ocean Territory,
                    Iraq,
                    Iran,
                    Iceland,
                    Italy,
                    Jersey,
                    Jamaica,
                    Jordan,
                    Japan,
                    Kenya,
                    Kyrgyzstan,
                    Cambodia,
                    Kiribati,
                    Comoros,
                    Saint Kitts and Nevis,
                    North Korea,
                    South Korea,
                    Kuwait,
                    Cayman Islands,
                    Kazakhstan,
                    Laos,
                    Lebanon,
                    Saint Lucia,
                    Liechtenstein,
                    Sri Lanka,
                    Liberia,
                    Lesotho,
                    Lithuania,
                    Luxembourg,
                    Latvia,
                    Libyan Arab Jamahiriya,
                    Morocco,
                    Monaco,
                    Moldova,
                    Montenegro,
                    Saint Martin,
                    Madagascar,
                    Marshall Islands,
                    Macedonia,
                    Mali,
                    Myanmar,
                    Mongolia,
                    Macao S.A.R. of China (PRC),
                    Northern Mariana Islands,
                    Martinique,
                    Mauritania,
                    Montserrat,
                    Malta,
                    Mauritius,
                    Maldives,
                    Malawi,
                    Mexico,
                    Malaysia,
                    Mozambique,
                    Namibia,
                    New Caledonia,
                    Niger,
                    Norfolk Island,
                    Nigeria,
                    Nicaragua,
                    Netherlands,
                    Norway,
                    Nepal,
                    Nauru,
                    Niue,
                    New Zealand,
                    Oman,
                    Panama,
                    Peru,
                    French Polynesia,
                    Papua New Guinea,
                    Philippines,
                    Pakistan,
                    Poland,
                    Saint Pierre and Miquelon,
                    Pitcairn,
                    Puerto Rico,
                    Palestinian Territory, Occupied,
                    Portugal,
                    Palau,
                    Paraguay,
                    Qatar,
                    Reunion,
                    Romania,
                    Serbia,
                    Russian Federation,
                    Rwanda,
                    Saudi Arabia,
                    Solomon Islands,
                    Seychelles,
                    Sudan,
                    Sweden,
                    Singapore,
                    Saint Helena,
                    Slovenia,
                    Svalbard and Jan Mayen,
                    Slovakia,
                    Sierra Leone,
                    San Marino,
                    Senegal,
                    Somalia,
                    Suriname,
                    Sao Tome and Principe,
                    El Salvador,
                    Sint Maarten,
                    Syrian Arab Republic,
                    Swaziland,
                    Turks and Caicos Islands,
                    Chad,
                    French Southern Territories,
                    Togo,
                    Thailand,
                    Tajikistan,
                    Tokelau,
                    Turkmenistan,
                    Tunisia,
                    Tonga,
                    East Timor,
                    Turkey,
                    Trinidad and Tobago,
                    Tuvalu,
                    Taiwan,
                    Tanzania, United Republic of,
                    Ukraine,
                    Uganda,
                    USA Minor Outlying Islands,
                    United States,
                    Uruguay,
                    Uzbekistan,
                    Vatican City State,
                    St Vincent and the Grenadines,
                    Venezuela,
                    Virgin Islands, British,
                    Virgin Islands, U.S.,
                    Viet Nam,
                    Vanuatu,
                    Wallis and Futuna,
                    Samoa,
                    Yemen,
                    Mayotte,
                    South Africa,
                    Zambia,
                    Zimbabwe
                ]
```
