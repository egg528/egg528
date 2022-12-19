# WebMvcConfigurer

### 왜 사용하는가?

- WebMvcConfigurer을 구현한 Config파일을 두면 @EnbleWebMvc 어노테이션이 자동으로 설정해주는 세팅값 + 원하는 세팅을 추가할 수 있다.



### @EnableWebMvc란?

- 정말 다양한 설정을 해준다

`@EnableWebMvc`

``` java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Import(DelegatingWebMvcConfiguration.class)
public @interface EnableWebMvc {
}
```


`DelegatingWebMvcConfiguration`

``` java
@Configuration(proxyBeanMethods = false)
public class DelegatingWebMvcConfiguration extends WebMvcConfigurationSupport {

	private final WebMvcConfigurerComposite configurers = new WebMvcConfigurerComposite();
 ...

}

```

`WebMvcConfigurationSupport`

``` java
public class WebMvcConfigurationSupport implements ApplicationContextAware, ServletContextAware {
	...

	/**
	 * Provide access to the shared handler interceptors used to configure
	 * {@link HandlerMapping} instances with.
	 * <p>This method cannot be overridden; use {@link #addInterceptors} instead.
	 */
	protected final Object[] getInterceptors(
			FormattingConversionService mvcConversionService,
			ResourceUrlProvider mvcResourceUrlProvider) {

		if (this.interceptors == null) {
			InterceptorRegistry registry = new InterceptorRegistry();
			addInterceptors(registry);
			registry.addInterceptor(new ConversionServiceExposingInterceptor(mvcConversionService));
			registry.addInterceptor(new ResourceUrlProviderExposingInterceptor(mvcResourceUrlProvider));
			this.interceptors = registry.getInterceptors();
		}
		return this.interceptors.toArray();
	}
  
  ...
}

```

- 