## SpringCloud版本兼容

#### 1.网址：

>https://start.spring.io/actuator/info

#### 2.表格:

| Spring Cloud Version        | Spring Cloud Alibaba Version      | Spring Boot Version |
| :-------------------------- | :-------------------------------- | :------------------ |
| Spring Cloud 2020.0.1       | 2021.1                            | 2.4.2               |
| Spring Cloud Hoxton.SR9     | 2.2.6.RELEASE                     | 2.3.2.RELEASE       |
| Spring Cloud Greenwich.SR6  | 2.1.4.RELEASE                     | 2.1.13.RELEASE      |
| Spring Cloud Hoxton.SR3     | 2.2.1.RELEASE                     | 2.2.5.RELEASE       |
| Spring Cloud Hoxton.RELEASE | 2.2.0.RELEASE                     | 2.2.X.RELEASE       |
| Spring Cloud Greenwich      | 2.1.2.RELEASE                     | 2.1.X.RELEASE       |
| Spring Cloud Finchley       | 2.0.4.RELEASE(停止维护，建议升级) | 2.0.X.RELEASE       |
| Spring Cloud Edgware        | 1.5.1.RELEASE(停止维护，建议升级) | 1.5.X.RELEASE       |

#### 3.版本对应关系:

```json
{
	"git": {
		"branch": "30890652847294a239a667375b5cb824cc1b0724",
		"commit": {
			"id": "3089065",
			"time": "2022-06-07T11:26:39Z"
		}
	},
	"build": {
		"version": "0.0.1-SNAPSHOT",
		"artifact": "start-site",
		"versions": {
			"spring-boot": "2.7.0",
			"initializr": "0.13.0-SNAPSHOT"
		},
		"name": "start.spring.io website",
		"time": "2022-06-07T11:28:30.322Z",
		"group": "io.spring.start"
	},
	"bom-ranges": {
		"codecentric-spring-boot-admin": {
			"2.4.3": "Spring Boot >=2.3.0.M1 and <2.5.0-M1",
			"2.5.6": "Spring Boot >=2.5.0.M1 and <2.6.0-M1",
			"2.6.5": "Spring Boot >=2.6.0.M1 and <2.7.0-M1"
		},
		"solace-spring-boot": {
			"1.1.0": "Spring Boot >=2.3.0.M1 and <2.6.0-M1",
			"1.2.1": "Spring Boot >=2.6.0.M1 and <2.7.0-M1"
		},
		"solace-spring-cloud": {
			"1.1.1": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
			"2.1.0": "Spring Boot >=2.4.0.M1 and <2.6.0-M1",
			"2.3.0": "Spring Boot >=2.6.0.M1 and <2.7.0-M1"
		},
		"spring-cloud": {
			"Hoxton.SR12": "Spring Boot >=2.2.0.RELEASE and <2.4.0.M1",
			"2020.0.5": "Spring Boot >=2.4.0.M1 and <2.6.0-M1",
			"2021.0.0-M1": "Spring Boot >=2.6.0-M1 and <2.6.0-M3",
			"2021.0.0-M3": "Spring Boot >=2.6.0-M3 and <2.6.0-RC1",
			"2021.0.0-RC1": "Spring Boot >=2.6.0-RC1 and <2.6.1",
			"2021.0.3": "Spring Boot >=2.6.1 and <3.0.0-M1",
			"2022.0.0-M1": "Spring Boot >=3.0.0-M1 and <3.0.0-M2",
			"2022.0.0-M2": "Spring Boot >=3.0.0-M2 and <3.1.0-M1"
		},
		"spring-cloud-azure": {
			"4.2.0": "Spring Boot >=2.5.0.M1 and <3.0.0-M1"
		},
		"spring-cloud-gcp": {
			"2.0.11": "Spring Boot >=2.4.0-M1 and <2.6.0-M1",
			"3.3.0": "Spring Boot >=2.6.0-M1 and <2.7.0-M1"
		},
		"spring-cloud-services": {
			"2.3.0.RELEASE": "Spring Boot >=2.3.0.RELEASE and <2.4.0-M1",
			"2.4.1": "Spring Boot >=2.4.0-M1 and <2.5.0-M1",
			"3.3.0": "Spring Boot >=2.5.0-M1 and <2.6.0-M1",
			"3.4.0": "Spring Boot >=2.6.0-M1 and <2.7.0-M1"
		},
		"spring-geode": {
			"1.3.12.RELEASE": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
			"1.4.13": "Spring Boot >=2.4.0-M1 and <2.5.0-M1",
			"1.5.14": "Spring Boot >=2.5.0-M1 and <2.6.0-M1",
			"1.6.8": "Spring Boot >=2.6.0-M1 and <2.7.0-M1",
			"1.7.0": "Spring Boot >=2.7.0-M1 and <3.0.0-M1",
			"2.0.0-M3": "Spring Boot >=3.0.0-M1 and <3.1.0-M1"
		},
		"vaadin": {
			"14.8.12": "Spring Boot >=2.1.0.RELEASE and <2.6.0-M1",
			"23.0.11": "Spring Boot >=2.6.0-M1 and <2.8.0-M1"
		},
		"wavefront": {
			"2.0.2": "Spring Boot >=2.1.0.RELEASE and <2.4.0-M1",
			"2.1.1": "Spring Boot >=2.4.0-M1 and <2.5.0-M1",
			"2.2.2": "Spring Boot >=2.5.0-M1 and <2.7.0-M1",
			"2.3.0": "Spring Boot >=2.7.0-M1 and <3.0.0-M1"
		}
	},
	"dependency-ranges": {
		"native": {
			"0.9.0": "Spring Boot >=2.4.3 and <2.4.4",
			"0.9.1": "Spring Boot >=2.4.4 and <2.4.5",
			"0.9.2": "Spring Boot >=2.4.5 and <2.5.0-M1",
			"0.10.0": "Spring Boot >=2.5.0-M1 and <2.5.2",
			"0.10.1": "Spring Boot >=2.5.2 and <2.5.3",
			"0.10.2": "Spring Boot >=2.5.3 and <2.5.4",
			"0.10.3": "Spring Boot >=2.5.4 and <2.5.5",
			"0.10.4": "Spring Boot >=2.5.5 and <2.5.6",
			"0.10.5": "Spring Boot >=2.5.6 and <2.5.9",
			"0.10.6": "Spring Boot >=2.5.9 and <2.6.0-M1",
			"0.11.0-M1": "Spring Boot >=2.6.0-M1 and <2.6.0-RC1",
			"0.11.0-M2": "Spring Boot >=2.6.0-RC1 and <2.6.0",
			"0.11.0-RC1": "Spring Boot >=2.6.0 and <2.6.1",
			"0.11.0": "Spring Boot >=2.6.1 and <2.6.2",
			"0.11.1": "Spring Boot >=2.6.2 and <2.6.3",
			"0.11.2": "Spring Boot >=2.6.3 and <2.6.4",
			"0.11.3": "Spring Boot >=2.6.4 and <2.6.6",
			"0.11.5": "Spring Boot >=2.6.6 and <2.7.0-M1",
			"0.12.0": "Spring Boot >=2.7.0-M1 and <3.0.0-M1"
		},
		"okta": {
			"1.4.0": "Spring Boot >=2.2.0.RELEASE and <2.4.0-M1",
			"1.5.1": "Spring Boot >=2.4.0-M1 and <2.4.1",
			"2.0.1": "Spring Boot >=2.4.1 and <2.5.0-M1",
			"2.1.5": "Spring Boot >=2.5.0-M1 and <2.7.0-M1"
		},
		"mybatis": {
			"2.1.4": "Spring Boot >=2.1.0.RELEASE and <2.5.0-M1",
			"2.2.2": "Spring Boot >=2.5.0-M1"
		},
		"camel": {
			"3.5.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
			"3.10.0": "Spring Boot >=2.4.0.M1 and <2.5.0-M1",
			"3.13.0": "Spring Boot >=2.5.0.M1 and <2.6.0-M1",
			"3.17.0": "Spring Boot >=2.6.0.M1 and <2.7.0-M1"
		},
		"picocli": {
			"4.6.3": "Spring Boot >=2.4.0.RELEASE and <3.0.0-M1"
		},
		"open-service-broker": {
			"3.2.0": "Spring Boot >=2.3.0.M1 and <2.4.0-M1",
			"3.3.1": "Spring Boot >=2.4.0-M1 and <2.5.0-M1",
			"3.4.1": "Spring Boot >=2.5.0-M1 and <2.6.0-M1",
			"3.5.0": "Spring Boot >=2.6.0-M1 and <2.7.0-M1"
		}
	}
}
```

