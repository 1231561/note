# ModelAndView的用法

#### addObject方法

该方法的作用是将所需的数据放到即将跳转的页面中去，作用相当于request的setAttribute方法。

##### setViewName方法

setViewName方法的参数是即将跳转的页面的名字，经过处理加上前缀和后缀即可得到完整的页面地址。

```java
@RequestMapping("wl_user_detail")
public ModelAndView showUserInfoDetail(HttpServletRequest request) {
    UserInfo userInfo = (UserInfo) request.getSession().getAttribute("userInfo");
    ModelAndView mv = new ModelAndView();
    UserInfoVO userInfoVO = new UserInfoVO();
    userInfoVO.setUserInfo(userInfo);
    String userType = UserDataChangeUtil.getUserType(userInfo.getUserType());
    userInfoVO.setTypeName(userType);
    String statusName = UserDataChangeUtil.getUserStatus(userInfo.getStatus());
    userInfoVO.setStatusName(statusName);
    if (userInfo != null) {
        mv.addObject("userInfo", userInfoVO);
        mv.setViewName("user/manage_user_detail");
    }
    return mv;
}
```