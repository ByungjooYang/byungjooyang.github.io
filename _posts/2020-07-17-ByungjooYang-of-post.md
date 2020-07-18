---
title:  "Spring MVC"
header:
  teaser: ""
categories: 
  - Spring
  - MVC
tags:
  - Spring
  - MVC
---

**★ Spring MVC**

스프링 MVC도 컨트롤러를 사용하여 클라이언트의 요청을 처리한다
스프링에서 DispatcherServlet 이 MVC에서 C(Control) 부분을 처리한다.
개발자가 처리할 부분은 클라이언트의 요청을 처리할 컨트롤러와 
응답화면을 전송할 JSP나 Velocity 템플릿 등 뷰 코드이다
DispatcherServlet, HandlerMapping, ViewResolver등은 
스프링이 기본적으로 제공하는 구현 클래스를 사용한다.

**MVC 구성요소**

1. DispatcherServlet
 : 클라이언트의 요청을 전달받음.
  컨트롤러에게 클라이언트 요청을 전달하고 Controller가 리턴한 결과값을 View에 전달하여 응답을 생성하도록 한다.

2. HandlerMapping
 : 클라이언트의 요청 URL을 어떤 Controller가 처리할지 결정.

3. Controller
 : 클라이언트의 요청을 처리한 뒤 결과를 DispatcherServlet에 알려준다.

4. ModelAndView
 : 컨트롤러가 처리한 결과 정보 및 뷰 선택에 필요한 정보를 담는다.

5. ViewResolver
 : 컨트롤러의 처리 결과를 생성할 뷰를 결정한다.

6. View
 : 컨트롤러의 처리 결과 화면을 생성한다.
  JSP나 Velocity 탬플릿 파일 등을 뷰로 사용한다.


**MVC 설정파일**

1. web.xml
 - DispatcherServlet 설정을 통해 스프링 설정 파일을 지정한다.
 - *.do 혹은 / 로 들어오는 클라이언트 요청을 dispatcherServlet이 처리하도록 한다.
 - DispatcherServlet은 /WEB-INF/서블릿명-serlvet.xml 파일로부터 스프링 설정 정보를 읽어온다.

 ```ruby
  <servlet>
  	<servlet-name>dispatcher</servlet-name>
  	<servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
  	<init-param>
  		<param-name>contextConfigLocation</param-name>
  		<param-value>
  		/WEB-INF/appServlet/mvc-context.xml
  		/WEB-INF/appServlet/servlet-context.xml
  		</param-value>
  	</init-param>
  </servlet> 
  
  <servlet-mapping>
  	<servlet-name>dispatcher</servlet-name>
  	<!-- <url-pattern>*.do</url-pattern> -->
  	<url-pattern>/</url-pattern>
  </servlet-mapping>

  //여기서는 param을 줘서 다른 context로 가서 설정하도록 한다.
 ```

2. Controller.java
 - 컨트롤러를 구현하려면 @Controller 어노테이션을 적용한다.
 - @RequestMapping 어노테이션을 이용해 클라이언트 요청을 처리할 메소드를 지정한다.
 - ModelAndView.setViewName()메소드를 이용해 컨트롤러의 처리 결과를 보여줄 뷰 이름을 지정한다.

 ```ruby
@Controller
public class SumController {
	
//	@RequestMapping(value="/input.do", method=RequestMethod.GET)
//	public ModelAndView input() {
//		ModelAndView mav = new ModelAndView();
//		mav.setViewName("/sum/input");
//		return mav;
//	}
	
	@RequestMapping(value="/input.do", method=RequestMethod.GET)
	//@ResponseBody 뷰의 이름이 아니라 실제 문자열로 리턴할때 쓴다.
	public String input() { //리턴 타입이 string이면 단순 문자열이 아니라 뷰의 이름으로 인식/사용된다. 여기선 jsp 파일 명으로 인식한당
		return "/sum/input";
	}
	
	
	
//	@RequestMapping(value="/result.do", method=RequestMethod.GET)
//	public ModelAndView result() {
//		ModelAndView mav = new ModelAndView();
//		mav.setViewName("/sum/result");
//		return mav;		
//	}
	
//	@RequestMapping(value="/result.do", method=RequestMethod.GET)
//	public ModelAndView result(@RequestParam(required=false, defaultValue="0") String x,  //=>입력값이 없을 때 값을 0으로 만들어 준다. 값을 int로 받는 것 보다 string으로 받는 것이 좋다. 
//							   @RequestParam(required=false, defaultValue="0") String y ) { // NumberFormatException 예방.
//		ModelAndView mav = new ModelAndView();
//		mav.addObject("x", x);
//		mav.addObject("y", y);
//		mav.setViewName("/sum/result");
//		return mav;		
//	}
	
//	@RequestMapping(value="/result.do", method=RequestMethod.GET) //맵으로 해도 되고 dto로 들어와도 된다. map은 모든 name을 키값으로 만들기때문에 특정 속성만 쓰고싶으면 dto로 잡으면된다.
//	public String result(@RequestParam Map<String, String> map, ModelMap modelMap) { //name 속성에 x라는 값이 들어오면 spring이 알아서 키 값을 x 로 만들어서 맵핑해준다.
//		modelMap.put("x", map.get("x"));
//		modelMap.put("y", map.get("y"));
//		return "/sum/result";		
//	}
	
	@RequestMapping(value="/result.do", method=RequestMethod.GET)
	public String result(@ModelAttribute SumDTO sumDTO, Model model) {
		model.addAttribute("sumDTO", sumDTO);
		return "/sum/result";		
  }
  
  //구현 방식은 아주 많으므로 알아서 선택해서 사용하면 된다.

 ```

3. dispatcher-Servlet.xml
 - DispatcherServlet은 스프링 컨테이너에서 컨트롤러 객체를 검색하기 때문에 스프링 설정 파일에 컨트롤러 빈으로 등록해줘야 한다.
 - 뷰 이름과 매칭되는 뷰 구현체를 찾기 위해 ViewResolver를 사용해야한다.
 - 스프링 MVC는 JSP, Velocity, FreeMarker등 뷰 구현 기술과의 연동을 지원하는데, JSP를 뷰 기술로 사용할 경우 InternalResourceViewResolver 구현체를 빈으로 등록해주면 된다.
 - ViewResolver가 /view/뷰이름.jsp를 뷰jsp로 사용한다는 의미.

 ```ruby
	<bean id="" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
		<property name="suffix" value=".jsp"/>
	</bean>
	
	<bean id="" class="com.controller.SumController"></bean>

 ```