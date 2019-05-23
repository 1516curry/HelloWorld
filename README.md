# HelloWorld
1516curry demo


system.out.println("hello curry");




package com.wukong.atp.framework.web.model;

import java.io.Serializable;

import lombok.Data;

/** .
 * 统一封装响应实体
 * 
 * @author chenl
 * 
 */
@Data
public class ResponseResultModel implements Serializable {

	/**
	 * serialVersionUID
	 */
	private static final long serialVersionUID = -1271969899669286075L;
	
	private String result;

}





=========================================================================================================================================================================================================






package com.wukong.atp.framework.web.model;

import java.io.Serializable;

import com.wukong.atp.framework.exception.GeneralBusinessException;

/**
 * . 返回前端数据封装实体
 * 
 * @param <T>
 *            .
 * @author chenl
 * 
 */
public class MsgBean<T> implements Serializable {

	private static final long serialVersionUID = 1448282180479342821L;

	private MsgCode code;

	private String detail;

	private T entity;
	
	private GeneralBusinessException generalBusinessException;
	
	public MsgBean() {
	}
	
	public MsgBean(MsgCode code) {
		this.code = code;
	}
	
	public MsgBean(MsgCode code, String detail) {
		this.code = code;
		this.detail = detail;
	}

	public int getCode() {
		switch (code) {
		    case SUCCESS:
			    // 成功
			    return 0;
		    case FAILURE:
			    // 正常失败
			    return 1;
		    case SYSTEM_ERROR:
			    // 系统内部错误
			    return 2;
		    case PARAM_INVALID:
			    // 参数错误
			    return 3;
		    case FILE_IMPORT_EXCEPTION:
			    // 文件导入错误
			    return 4;
		    case NEED_LOGIN:
			    // 需要登录
			    return 5;
		    case LACK_OF_PUBLIC_PARAM:
			    // 缺少公共参数
			    return 6;
		    case SIGNATURE_INVALID:
			    // 签名错误
			    return 7;
		    default:
			    return 0;
		}
	}

	public MsgBean<T> setCode(MsgCode code) {
		this.code = code;
		return this;
	}

	public String getDetail() {
		return this.detail;
	}

	public MsgBean<T> setDetail(String detail) {
		this.detail = detail;
		return this;
	}

	/**
	 * 
	 * @ClassName Code
	 * @Description Code枚举
	 * @author:01371262
	 * @Date 2018年2月1日 上午9:34:49
	 * @version 1.0.0
	 */
	public enum MsgCode {
		// 成功
		SUCCESS(0),
		// 正常失败
		FAILURE(1),
		// 系统内部错误
		SYSTEM_ERROR(2),
		// 参数错误
		PARAM_INVALID(3),
		// 文件导入错误
		FILE_IMPORT_EXCEPTION(4),
		// 需要登录
		NEED_LOGIN(5),
		// 缺少公共参数
		LACK_OF_PUBLIC_PARAM(6),
		// 签名错误
		SIGNATURE_INVALID(7);
		
		private int code;

		MsgCode(int code) {
			this.code = code;
		}

		public int getCode() {
			return code;
		}
	}

	public T getEntity() {
		return entity;
	}

	public MsgBean<T> setEntity(T entity) {
		this.entity = entity;
		return this;
	}
	
	public GeneralBusinessException getGeneralBusinessException() {
		return generalBusinessException;
	}

	public void setGeneralBusinessException(GeneralBusinessException generalBusinessException) {
		this.generalBusinessException = generalBusinessException;
	}

}




=========================================================================================================================================================================================================





package com.wukong.atp.framework.web.controller;

import java.util.List;
import java.util.Locale;

import javax.servlet.http.HttpServletRequest;

import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.MessageSource;
import org.springframework.context.i18n.LocaleContextHolder;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.ResponseBody;

import com.alibaba.fastjson.JSONObject;
import com.wukong.atp.framework.exception.ApplicationServiceException;
import com.wukong.atp.framework.exception.GeneralBusinessException;
import com.wukong.atp.framework.utils.EncryptUtils;
import com.wukong.atp.framework.utils.JsonUtils;
import com.wukong.atp.framework.utils.ObjectUtils;
import com.wukong.atp.framework.web.context.ContextHolder;
import com.wukong.atp.framework.web.context.DataPermission;
import com.wukong.atp.framework.web.context.UserInfo;
import com.wukong.atp.framework.web.handler.BaseInvokeChainHandler;
import com.wukong.atp.framework.web.model.MsgBean;
import com.wukong.atp.framework.web.model.MsgBean.MsgCode;
import com.wukong.atp.framework.web.model.RequestParamModel;
import com.wukong.atp.framework.web.model.ResponseResultModel;
import com.wukong.atp.framework.web.param.BaseParam;

/**
 * . 基础Controller
 * 
 * @author chenl
 * 
 */
public class BaseController extends BaseInvokeChainHandler {

	protected static final Logger LOGGER = LoggerFactory.getLogger(BaseController.class);

	private static final String COMMON_EXCEPTION_TIPS = "系统正在维护中，请稍候.....";

	@Autowired
	protected MessageSource messageSource;

	/**
	 * 获取前台传给后端的参数
	 * 
	 * @param req
	 *            http请求
	 * @return RequestParamModel 参数对象
	 */
	protected RequestParamModel getParameter(HttpServletRequest req) {
		return (RequestParamModel) req.getAttribute("param");
	}

	/**
	 * 获取前台传给后端的参数实体封装类实体
	 * 
	 * @param req
	 *            http请求
	 * @param clazz
	 *            类定义
	 * @return Q 类实体
	 */
	protected <Q extends BaseParam> Q getEntityParam(HttpServletRequest req, Class<Q> clazz) {
		JSONObject entity = this.getParameter(req).getEntity();
		Q obj;
		if (ObjectUtils.isNullOrEmpty(entity)) {
			String msg;
			try {
				obj = clazz.newInstance();
			} catch (Exception e) {
				msg = "系统出现异常";
				throw new IllegalArgumentException(msg);
			}
		} else {
			obj = JsonUtils.parseObject(entity, clazz);
		}
		obj.validateParams();
		return obj;
	}

	/**
	 * 封装返回结果
	 * 
	 * @param code
	 *            消息编码
	 * @param detail
	 *            消息内容
	 * @return ResponseResultModel 结果参数对象
	 */
	protected ResponseResultModel encapsulateResult(MsgCode code, String detail) {
		return encapsulateResult(null, code, detail);
	}

	/**
	 * 封装返回结果
	 * 
	 * @param <T>
	 * 
	 * @param result
	 *            返回结果对象
	 * @param code
	 *            消息编码
	 * @param detail
	 *            消息内容
	 * @return ResponseResultModel 结果参数对象
	 */
	protected <T> ResponseResultModel encapsulateResult(T result, MsgCode code, String detail) {
		MsgBean<T> msgBean = new MsgBean<>();
		if (!ObjectUtils.isNullOrEmpty(result)) {
			msgBean.setEntity(result);
		}
		msgBean.setCode(code).setDetail(detail);
		ResponseResultModel responseResultModel = new ResponseResultModel();
		String oriResultString = JsonUtils.toJsonStringWithDateFormat(msgBean);
		LOGGER.info("============返回原始数据：" + oriResultString);
		responseResultModel.setResult(EncryptUtils.base64Encode(oriResultString));
		return responseResultModel;
	}
	
	/**
	 * 封装返回结果
	 * 
	 * @param msgBean
	 *            msgBean封装实体
	 * @return ResponseResultModel 结果参数对象
	 */
	protected <T> ResponseResultModel encapsulateResult(MsgBean<T> msgBean) {
		ResponseResultModel responseResultModel = new ResponseResultModel();
		String oriResultString = JsonUtils.toJsonStringWithDateFormat(msgBean);
		LOGGER.info("============返回原始数据：" + oriResultString);
		responseResultModel.setResult(EncryptUtils.base64Encode(oriResultString));
		return responseResultModel;
	}

	/**
	 * 获取国际化信息
	 * 
	 * @param messageKey
	 *            messages资源文件里的Key
	 * @return String messages资源文件里相应Key对应的值
	 */
	protected String getMessage(String messageKey) {
		return messageSource.getMessage(messageKey, null, getLocale());
	}

	/**
	 * 获取国际化信息
	 * 
	 * @param messageKey
	 *            messages资源文件里的Key
	 * @param args
	 *            messages资源文件里的Key对应值的参数
	 * @return String messages资源文件里相应Key对应的值
	 */
	protected String getMessage(String messageKey, Object[] args) {
		return messageSource.getMessage(messageKey, args, getLocale());
	}

	/**
	 * 获取Locale
	 * 
	 * @return Locale
	 */
	private Locale getLocale() {
		return LocaleContextHolder.getLocale();
	}
	
	/**
	 * 获取用户信息
	 * 
	 * @return UserInfo 用户信息
	 */
	protected UserInfo getUserInfo() {
		return ContextHolder.get().getUser();
	}
	
	/**
	 * 获取用户ID
	 * 
	 * @return String 用户ID
	 */
	protected String getUserId() {
		String userId = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			userId = userInfo.getUserId();
		}
		return userId;
	}
	
	/**
	 * 获取用户名
	 * 
	 * @return String 用户名
	 */
	protected String getUserName() {
		String userName = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			userName = userInfo.getUserName();
		}
		return userName;
	}
	
	/**
	 * 获取用户账号
	 * 
	 * @return String 用户账号
	 */
	protected String getAccount() {
		String account = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			account = userInfo.getAccount();
		}
		return account;
	}
	
	/**
	 * 获取企业编码
	 * 
	 * @return String 企业编码
	 */
	protected String getEnterpriseCode() {
		String enterpriseCode = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			enterpriseCode = userInfo.getEnterpriseCode();
		}
		return enterpriseCode;
	}

	/**
	 * 获取所属集团公司编码
	 *
	 * @return String 所属集团公司编码
	 */
	protected String getGroupEnterpriseCode() {
		String groupEnterpriseCode = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			groupEnterpriseCode = userInfo.getGroupEnterpriseCode();
		}
		return groupEnterpriseCode;
	}
	
	/**
	 * 获取用户数据权限
	 * 
	 * @return List<DataPermission> 用户数据权限
	 */
	protected List<DataPermission> getDataPermissions() {
		String userDataPermissions = null;
		List<DataPermission> dataPermissionList = null;
		UserInfo userInfo =  ContextHolder.get().getUser();
		if (!ObjectUtils.isNullOrEmpty(userInfo)) {
			userDataPermissions = userInfo.getUserDataPermissions();
			if (!ObjectUtils.isNullOrEmpty(userDataPermissions)) {
				dataPermissionList = JsonUtils.parserToList(userDataPermissions, DataPermission.class);
			}
		}
		return dataPermissionList;
	}

	/**
	 * 请求接口异常统一处理
	 *
	 * @param request
	 *            http请求
	 * @param e
	 *            异常对象
	 * @return ResponseResultModel 结果参数对象
	 */
	@ExceptionHandler(value = { Exception.class })
	@ResponseBody
	public ResponseResultModel handleException(HttpServletRequest request, Exception e) {
		ResponseResultModel result = null;
		if (e instanceof GeneralBusinessException 
			   || e instanceof IllegalArgumentException
			   || e instanceof IllegalStateException) {
			LOGGER.error(e.getMessage(), e);
			result = encapsulateResult(MsgBean.MsgCode.FAILURE, e.getMessage());
		} else if (e instanceof ApplicationServiceException) {
			if (e.getCause() instanceof GeneralBusinessException) {
				LOGGER.error(e.getCause().getMessage(), e);
				result = encapsulateResult(MsgBean.MsgCode.FAILURE, e.getCause().getMessage());
			} else if (e.getCause() instanceof IllegalArgumentException || e.getCause() instanceof IllegalStateException) {
				LOGGER.error(e.getCause().getMessage(), e);
				result = encapsulateResult(MsgBean.MsgCode.FAILURE, e.getCause().getMessage());
			} else {
				LOGGER.error("系统出现异常 : " + e.getCause().getMessage(), e);
				result = encapsulateResult(MsgBean.MsgCode.FAILURE, COMMON_EXCEPTION_TIPS);
			}
		} else {
			LOGGER.error("系统出现异常 : " + e.getMessage(), e);
			result = encapsulateResult(MsgBean.MsgCode.FAILURE, COMMON_EXCEPTION_TIPS);
		}
//		try {
//			result = (ResponseResultModel) super.getResponseResult(result);
//			return result;
//		} catch (Exception e1) {
//			return result;
//		}
		return result;
	}

}


























