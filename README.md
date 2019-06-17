
SET FOREIGN_KEY_CHECKS=0;

-- ----------------------------
-- Table structure for fin_account_card	结算卡号
-- ----------------------------
DROP TABLE IF EXISTS `fin_account_card`;
CREATE TABLE `fin_account_card` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `account_card_code` varchar(32) COLLATE utf8_bin NOT NULL COMMENT '结算帐号编码(客户编码-序列号，目前结算帐号与客户关联，可以使用客户编码；如果与合同关联，可以使用客户编码+合同编码)',
  `account_card_name` varchar(32) COLLATE utf8_bin DEFAULT NULL COMMENT '结算账号描述(客户名称)',
  `account_type` tinyint(2) NOT NULL COMMENT '账期类型  0:自然月；1:非自然月；2:半月结 3：自定义 4：自然周',
  `account_type_name` varchar(16) COLLATE utf8_bin NOT NULL COMMENT '账期类型描述：前端显示给用户看',
  `start_date` decimal(8,0) NOT NULL COMMENT '帐单生效开始日期。第一期账单的开始时间，用作计算账单的时间范围起点。',
  `accout_date_number` decimal(8,0) NOT NULL COMMENT '结算天数:自然月，不用录入 非自然月，不用录入 半月结，不用录入 其他自定义 时间，比如 周结 采用自定义，7 ',
  `account_number_owner` varchar(32) COLLATE utf8_bin NOT NULL COMMENT '结算账号归属用户(只有本用户才可以修改相关数据)',
  `current_start_date` decimal(8,0) NOT NULL COMMENT '当前账单开始日期（开始时使用账单开始时间，以后每次计算，不允许修改）',
  `current_end_date` decimal(8,0) NOT NULL COMMENT '当前账单截止日期（开始时可以使用账单开始时间，进行计算得出，用户可以修改，表示计算时间在第二天0点）',
  `pay_object` varchar(16) COLLATE utf8_bin NOT NULL COMMENT '结算对象归属网点编码',
  `pay_object_name` varchar(32) COLLATE utf8_bin DEFAULT NULL COMMENT '结算对象归属网点描述',
  `pay_type` tinyint(2) DEFAULT NULL COMMENT '付款方式（0：银行转账）',
  `agent_bank_name` varchar(32) COLLATE utf8_bin DEFAULT NULL COMMENT '付款对象银行账号名',
  `bank_name` varchar(32) COLLATE utf8_bin DEFAULT NULL COMMENT '付款对象银行名称(可以不录入)',
  `agent_bank_code` varchar(19) COLLATE utf8_bin DEFAULT NULL COMMENT '付款对象银行账号（可以不录入）',
  `bill_pay_object_name` varchar(32) COLLATE utf8_bin DEFAULT NULL COMMENT '付款对象名称',
  `transfer_pay_accout` varchar(19) COLLATE utf8_bin DEFAULT NULL COMMENT '代付账号(对应货代设置的支付平台账号，可以不录入)',
  `recieve_account_display` varchar(64) COLLATE utf8_bin NOT NULL COMMENT '支付账号账号显示(在客户支付时显示 供应商名称 )',
  `is_notified_agent` tinyint(2) NOT NULL DEFAULT '0' COMMENT '账单是否通知货代',
  `phone` varchar(16) COLLATE utf8_bin DEFAULT NULL COMMENT '货代联系电话',
  `is_notify_customer` tinyint(2) NOT NULL DEFAULT '0' COMMENT '是否通知客户',
  `customer_phone` varchar(16) COLLATE utf8_bin DEFAULT NULL COMMENT '客户联系电话',
  `limit_date_number` decimal(2,0) NOT NULL COMMENT '付款限制天数',
  `created_by` varchar(32) COLLATE utf8_bin NOT NULL COMMENT '创建者',
  `creation_date` datetime NOT NULL COMMENT '创建时间',
  `updated_by` varchar(32) COLLATE utf8_bin NOT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(2) NOT NULL COMMENT '是否删除',
  `plat_company_code` varchar(32) COLLATE utf8_bin NOT NULL,
  `memo` varchar(256) COLLATE utf8_bin DEFAULT NULL COMMENT '备注',
  `new_account_type` tinyint(2) DEFAULT NULL COMMENT '修改的账期类型  0:自然月；1:非自然月；2:半月结 3：自定义；4：自然周',
  `new_accout_date_number` decimal(8,0) DEFAULT NULL COMMENT '修改的结算天数',
  `new_start_date` decimal(8,0) DEFAULT NULL COMMENT '修改的账单开始日期(非自然月)',
  `invoice_type` tinyint(4) DEFAULT NULL COMMENT '发票类型：1-增值税普通发票 2-增值税专用发票 3-不开发票 4-电子普通发票',
  `customer_type` tinyint(4) DEFAULT '0' COMMENT '客户类型：0-代收代付,1-联盟客户;',
  `tax_rate` decimal(8,2) DEFAULT NULL COMMENT '发票税率',
  `business_type` tinyint(4) DEFAULT '0' COMMENT '卡号业务类型:0-进出港;1-出港;2-进港;',
  PRIMARY KEY (`id`),
  UNIQUE KEY `AK_uk_fin_account_card` (`account_card_code`,`plat_company_code`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='结算账号信息表';




-- ----------------------------
-- Table structure for fin_agent_bil	账单表
-- ----------------------------
DROP TABLE IF EXISTS `fin_agent_bil`;
CREATE TABLE `fin_agent_bil` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `bill_type` tinyint(4) DEFAULT NULL COMMENT '0:月结账单; 1:散单当日汇总; 2:散单月汇总;',
  `bill_code` varchar(64) DEFAULT NULL COMMENT '账单编码：月结：货代结算单位编码+客户编码+结算日期范围  散单：平台公司编码+客户对应网点+结算日期',
  `account_card_code` varchar(32) DEFAULT NULL COMMENT '结算帐号编码(客户编码-序列号，目前结算帐号与客户关联，可以使用客户编码；如果与合同关联，可以使用客户编码+合同编码)',
  `account_card_name` varchar(32) DEFAULT NULL COMMENT '结算账号描述',
  `current_start_date` decimal(8,0) DEFAULT NULL COMMENT '当前账单开始日期（开始时使用账单开始时间，以后每次计算，不允许修改）',
  `current_end_date` decimal(8,0) DEFAULT NULL COMMENT '当前账单截止日期（开始时可以使用账单开始时间，进行计算得出，用户可以修改，表示计算时间在第二天0点）',
  `pay_object` varchar(16) DEFAULT NULL COMMENT '结算对象归属网点编码',
  `pay_object_name` varchar(32) DEFAULT NULL COMMENT '结算对象归属网点描述',
  `due_account_amount` decimal(10,2) DEFAULT '0.00' COMMENT '应收款金额（如果是散单，为0 不显示）',
  `actual_receive_amount` decimal(10,2) DEFAULT '0.00' COMMENT '实收款总金额',
  `due_pay_amount` decimal(10,2) DEFAULT '0.00' COMMENT '应代付总金额\n            目前等于实收款金额',
  `actual_pay_amount` decimal(10,2) DEFAULT '0.00' COMMENT '实际代付总金额 实际代付信息汇总\n            ',
  `receive_status` tinyint(4) DEFAULT '0' COMMENT '0 表示未收款; 1表示部分收款;2 表示全额收款\n            散单汇总 2 表示全额支付\n            ',
  `pay_status` tinyint(4) DEFAULT NULL COMMENT '0 表示未客户代付 1 表示部分客户代付 2 表示全部代付',
  `status` tinyint(4) DEFAULT '0' COMMENT '0 表示初始状态（未确认）; 1 表示客户确定金额（确认，可以进行收款） 2 表示用户确定账单完成，不再处理。',
  `calc_date` datetime DEFAULT NULL COMMENT '账单生成时间',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `so_collect_date` decimal(8,0) DEFAULT NULL COMMENT '散单汇总日期',
  `plat_company_code` varchar(32) NOT NULL,
  `receive_freeze_amount` decimal(10,2) DEFAULT '0.00' COMMENT '收款冻结金额',
  `pay_freeze_amount` decimal(10,2) DEFAULT '0.00' COMMENT '付款冻结金额',
  `invoice_type` tinyint(4) DEFAULT NULL COMMENT '发票类型：1-增值税普通发票 2-增值税专用发票 3-不开票',
  `is_fengyi` tinyint(4) DEFAULT '0' COMMENT '是否丰羿  0：否  1：是',
  `version` tinyint(4) NOT NULL DEFAULT '1' COMMENT '版本号，用来控制并发操作提现金额',
  `customer_due_account_amount` decimal(10,2) DEFAULT '0.00' COMMENT '货主查看账单的总金额',
  `data_source` tinyint(4) DEFAULT NULL,
  PRIMARY KEY (`id`),
  UNIQUE KEY `fin_agent_bil_bill_code_uindex` (`bill_code`,`plat_company_code`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;




-- ----------------------------
-- Table structure for fin_agent_bil_detail	账单明细表
-- ----------------------------
DROP TABLE IF EXISTS `fin_agent_bil_detail`;
CREATE TABLE `fin_agent_bil_detail` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `bill_id` bigint(20) NOT NULL COMMENT '账单表序列号',
  `due_account_amount` decimal(10,2) DEFAULT NULL COMMENT '应付款金额（如果是散单，为0 不显示）',
  `actual_receive_amount` decimal(10,2) DEFAULT NULL COMMENT '实收款总金额',
  `bill_number` varchar(32) DEFAULT NULL COMMENT '分单号,与平台公司编码作为组合唯一键',
  `order_no` varchar(16) DEFAULT NULL COMMENT '订单号',
  `origin_city` char(6) DEFAULT NULL COMMENT '出发城市,城市六字码',
  `dest_city` char(6) DEFAULT NULL COMMENT '到达城市,城市六字码',
  `origin_city_name` varchar(32) DEFAULT NULL COMMENT '出发城市名称',
  `dest_city_name` varchar(32) DEFAULT NULL COMMENT '到达城市名称',
  `consignor_date` date DEFAULT NULL COMMENT '寄件日期',
  `consignor` varchar(32) DEFAULT NULL COMMENT '托运人',
  `consignor_phone` varchar(16) DEFAULT NULL COMMENT '托运联系电话',
  `consignor_addr_region` char(6) DEFAULT NULL COMMENT '托运地址区域',
  `consignor_detail_addr` varchar(128) DEFAULT NULL COMMENT '托运详细地址',
  `consignor_addr_zip` varchar(16) DEFAULT NULL COMMENT '托运地址邮编',
  `consignee` varchar(32) DEFAULT NULL COMMENT '收货人',
  `consignee_phone` varchar(16) DEFAULT NULL COMMENT '收货联系电话',
  `consignee_addr_region` char(6) DEFAULT NULL COMMENT '收货地址区域',
  `consignee_detail_addr` varchar(128) DEFAULT NULL COMMENT '收货详细地址',
  `consignee_addr_zip` varchar(16) DEFAULT NULL COMMENT '收货地址邮编',
  `pickup_type` varchar(16) DEFAULT NULL COMMENT '提货方式',
  `pickup_type_name` varchar(32) DEFAULT NULL COMMENT '提货方式名称',
  `pickup_contact_addr` varchar(256) DEFAULT NULL COMMENT '提货联系地址',
  `pickup_contact_phone` varchar(16) DEFAULT NULL COMMENT '提货联系电话',
  `payment_type` varchar(16) DEFAULT NULL COMMENT '付款方式',
  `pickup_contact` varchar(32) DEFAULT NULL COMMENT '提货联系人',
  `cargo_type` varchar(16) DEFAULT NULL COMMENT '货物类别',
  `cargo_type_name` varchar(32) DEFAULT NULL COMMENT '货物类别名称',
  `cargo_name` varchar(256) DEFAULT NULL COMMENT '货物品名',
  `pkgs` decimal(8,0) DEFAULT NULL COMMENT '件数',
  `cross_weight` decimal(8,2) DEFAULT NULL COMMENT '毛重',
  `charged_weight` decimal(8,2) DEFAULT NULL COMMENT '计费重量',
  `freight_rate` decimal(8,2) DEFAULT NULL COMMENT '费率',
  `flight_no` varchar(32) DEFAULT NULL COMMENT '航班号',
  `flight_date` date DEFAULT NULL COMMENT '航班日期',
  `flight_fee` decimal(8,2) DEFAULT NULL COMMENT '航空运费',
  `ground_fee` decimal(8,2) DEFAULT NULL COMMENT '地面运费',
  `delivery_fee` decimal(8,2) DEFAULT NULL COMMENT '派送费',
  `insurance_dec_value` decimal(10,2) DEFAULT NULL COMMENT '保险声明价值',
  `insurance_fee` decimal(8,2) DEFAULT NULL COMMENT '保险费',
  `fuel_fee` decimal(8,2) DEFAULT NULL COMMENT '燃油费',
  `other_fee` decimal(8,2) DEFAULT NULL COMMENT '其他费用',
  `total_fee` decimal(8,2) DEFAULT NULL COMMENT '运费总额',
  `collect_cargo_fee` decimal(8,2) DEFAULT NULL COMMENT '代收货款',
  `customer_wno` varchar(16) DEFAULT NULL COMMENT '客户单号',
  `account_card_no` varchar(32) DEFAULT NULL COMMENT '月结卡号',
  `is_back_signed` tinyint(1) DEFAULT NULL COMMENT '是否签回单',
  `business_type` varchar(16) DEFAULT NULL COMMENT '业务类型',
  `business_type_name` varchar(32) DEFAULT NULL COMMENT '业务类型名称',
  `payment_type_name` varchar(32) DEFAULT NULL COMMENT '付款方式名称',
  `memo` varchar(256) DEFAULT NULL COMMENT '备注',
  `bill_tm` datetime DEFAULT NULL COMMENT '开单时间',
  `bill_maker` varchar(32) DEFAULT NULL COMMENT '开单人',
  `bill_branch_name` varchar(64) DEFAULT NULL COMMENT '开单网点名称',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `version` int(11) DEFAULT NULL COMMENT '版本号',
  `plat_company_code` varchar(32) NOT NULL,
  `due_pay_amount` decimal(10,2) DEFAULT NULL COMMENT '应代付总金额\n            目前等于应收款金额',
  `actual_pay_amount` decimal(10,2) DEFAULT NULL COMMENT '实际代付总金额 实际代付信息汇总\n            ',
  `receive_status` tinyint(4) DEFAULT '0' COMMENT '0 表示未收款; 1表示部分收款;2 表示全额收款\\n            散单汇总 2 表示全额支付\\n            ',
  `pay_status` tinyint(4) DEFAULT NULL COMMENT '0 表示未客户代付 1 表示部分客户代付 2 表示全部代付',
  `pickup_fee` decimal(8,2) DEFAULT NULL COMMENT '提货费',
  `collect_fee` decimal(8,2) DEFAULT NULL COMMENT '到付金额',
  `partner_name` varchar(64) DEFAULT NULL COMMENT '代理名称',
  `is_internal_business` tinyint(1) DEFAULT NULL COMMENT '是否内部业务',
  `sign_man` varchar(32) DEFAULT NULL COMMENT '签收人',
  `signer_phone` varchar(16) DEFAULT NULL COMMENT '签收人联系电话',
  `id_card_no` varchar(32) DEFAULT NULL COMMENT '身份证号',
  `sign_tm` datetime DEFAULT NULL COMMENT '签收时间',
  `pickup_branch_name` varchar(64) DEFAULT NULL COMMENT '提货网点名称',
  `pickup_time` datetime DEFAULT NULL COMMENT '提货时间',
  `data_source` tinyint(4) DEFAULT NULL COMMENT '数据来源：出港0，进港1',
  `record_type` tinyint(4) DEFAULT '0' COMMENT '记录类型: 0-原单;1-冲单;2-补单',
  `customer_id` bigint(20) DEFAULT NULL COMMENT '下单客户',
  `customer_name` varchar(32) CHARACTER SET utf8 COLLATE utf8_bin DEFAULT NULL COMMENT '客户名称',
  `origin_id` bigint(20) DEFAULT NULL COMMENT '源表分单id',
  `door_pickup_fee` decimal(8,2) DEFAULT NULL COMMENT '揽收费',
  `status` tinyint(4) DEFAULT '0' COMMENT '0 表示未确认; 1 表示已确认',
  `is_fengyi` tinyint(4) DEFAULT '0' COMMENT '是否丰羿  0：否  1：是',
  `recieve_fee` decimal(8,2) DEFAULT NULL COMMENT '寄付运费',
  `collect_other_fee` decimal(8,2) DEFAULT NULL COMMENT '到付其他',
  `saas_total_fee` decimal(8,2) DEFAULT NULL COMMENT 'SAAS运费总额',
  `is_forwarder_hawb` tinyint(4) DEFAULT NULL COMMENT '是否货代分单',
  `receive_add` decimal(8,2) DEFAULT NULL COMMENT '应收增值费用',
  PRIMARY KEY (`id`),
  KEY `fin_agent_bil_detail_bill_id_index` (`bill_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;



-- ----------------------------
-- Table structure for fin_hawb_bil_extra_fee	账单明细费用表
-- ----------------------------
DROP TABLE IF EXISTS `fin_hawb_bil_extra_fee`;
CREATE TABLE `fin_hawb_bil_extra_fee` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `bill_detail_id` bigint(20) NOT NULL COMMENT '账单详情表序列号',
  `fee_name` varchar(32) DEFAULT NULL COMMENT '费用名称',
  `price_type` varchar(16) DEFAULT NULL COMMENT '计价方式',
  `price_type_name` varchar(32) DEFAULT NULL COMMENT '计价方式名称',
  `price_units` decimal(8,0) DEFAULT NULL COMMENT '计价单位量',
  `freight_rate` decimal(8,2) DEFAULT NULL COMMENT '费率',
  `fee_value` decimal(8,2) DEFAULT NULL COMMENT '费用值',
  `minum_freight` decimal(8,2) DEFAULT NULL COMMENT '最低收费',
  `memo` varchar(256) DEFAULT NULL COMMENT '备注',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `plat_company_code` varchar(32) NOT NULL,
  `fee_type` varchar(16) DEFAULT NULL COMMENT '费用编码',
  `data_source` tinyint(4) DEFAULT NULL COMMENT '数据来源：出港0，进港1',
  `fee_type_name` varchar(32) DEFAULT NULL COMMENT '费用编码名称',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;




-- ----------------------------
-- Table structure for fin_hawb_info	分单表
-- ----------------------------
DROP TABLE IF EXISTS `fin_hawb_info`;
CREATE TABLE `fin_hawb_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `bill_number` varchar(32) DEFAULT NULL COMMENT '分单号,与平台公司编码作为组合唯一键',
  `order_no` varchar(64) DEFAULT NULL COMMENT '订单号',
  `origin_city` char(6) DEFAULT NULL COMMENT '出发城市,城市六字码',
  `dest_city` char(6) DEFAULT NULL COMMENT '到达城市,城市六字码',
  `origin_city_name` varchar(32) DEFAULT NULL COMMENT '出发城市名称',
  `dest_city_name` varchar(32) DEFAULT NULL COMMENT '到达城市名称',
  `consignor_date` date DEFAULT NULL COMMENT '寄件日期',
  `consignor` varchar(32) DEFAULT NULL COMMENT '托运人',
  `consignor_phone` varchar(16) DEFAULT NULL COMMENT '托运联系电话',
  `consignor_addr_region` char(6) DEFAULT NULL COMMENT '托运地址区域',
  `consignor_detail_addr` varchar(128) DEFAULT NULL COMMENT '托运详细地址',
  `consignor_addr_zip` varchar(16) DEFAULT NULL COMMENT '托运地址邮编',
  `consignee` varchar(32) DEFAULT NULL COMMENT '收货人',
  `consignee_phone` varchar(16) DEFAULT NULL COMMENT '收货联系电话',
  `consignee_addr_region` char(6) DEFAULT NULL COMMENT '收货地址区域',
  `consignee_detail_addr` varchar(128) DEFAULT NULL COMMENT '收货详细地址',
  `consignee_addr_zip` varchar(16) DEFAULT NULL COMMENT '收货地址邮编',
  `pickup_type` varchar(16) DEFAULT NULL COMMENT '提货方式',
  `pickup_type_name` varchar(32) DEFAULT NULL COMMENT '提货方式名称',
  `pickup_contact_addr` varchar(256) DEFAULT NULL COMMENT '提货联系地址',
  `pickup_contact_phone` varchar(16) DEFAULT NULL COMMENT '提货联系电话',
  `payment_type` varchar(16) DEFAULT NULL COMMENT '付款方式',
  `payment_type_name` varchar(32) DEFAULT NULL COMMENT '付款方式名称',
  `pickup_contact` varchar(32) DEFAULT NULL COMMENT '提货联系人',
  `cargo_type` varchar(16) DEFAULT NULL COMMENT '货物类别',
  `cargo_type_name` varchar(32) DEFAULT NULL COMMENT '货物类别名称',
  `cargo_name` varchar(256) DEFAULT NULL COMMENT '货物品名',
  `pkgs` decimal(8,0) DEFAULT NULL COMMENT '件数',
  `cross_weight` decimal(8,2) DEFAULT NULL COMMENT '毛重',
  `charged_weight` decimal(8,2) DEFAULT NULL COMMENT '计费重量',
  `freight_rate` decimal(8,2) DEFAULT NULL COMMENT '费率',
  `flight_no` varchar(32) DEFAULT NULL COMMENT '航班号',
  `flight_date` date DEFAULT NULL COMMENT '航班日期',
  `flight_fee` decimal(8,2) DEFAULT NULL COMMENT '航空运费',
  `ground_fee` decimal(8,2) DEFAULT NULL COMMENT '地面运费',
  `delivery_fee` decimal(8,2) DEFAULT NULL COMMENT '派送费',
  `insurance_dec_value` decimal(10,2) DEFAULT NULL COMMENT '保险声明价值',
  `insurance_fee` decimal(8,2) DEFAULT NULL COMMENT '保险费',
  `fuel_fee` decimal(8,2) DEFAULT NULL COMMENT '燃油费',
  `other_fee` decimal(8,2) DEFAULT NULL COMMENT '其他费用',
  `total_fee` decimal(8,2) DEFAULT NULL COMMENT '运费总额',
  `collect_cargo_fee` decimal(8,2) DEFAULT NULL COMMENT '代收货款',
  `customer_wno` varchar(16) DEFAULT NULL COMMENT '客户单号',
  `account_card_no` varchar(32) DEFAULT NULL COMMENT '月结卡号',
  `is_back_signed` tinyint(1) DEFAULT NULL COMMENT '是否签回单',
  `business_type` varchar(16) DEFAULT NULL COMMENT '业务类型',
  `business_type_name` varchar(32) DEFAULT NULL COMMENT '业务类型名称',
  `memo` varchar(256) DEFAULT NULL COMMENT '备注',
  `bill_tm` datetime DEFAULT NULL COMMENT '开单时间',
  `bill_maker` varchar(32) DEFAULT NULL COMMENT '开单人',
  `bill_branch_name` varchar(64) DEFAULT NULL COMMENT '开单网点名称',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `version` int(11) DEFAULT NULL COMMENT '版本号',
  `plat_company_code` varchar(32) NOT NULL,
  `due_account_amount` decimal(10,2) DEFAULT NULL COMMENT '应付款金额（如果是散单，为0 不显示）',
  `actual_receive_amount` decimal(10,2) DEFAULT NULL COMMENT '实收款总金额',
  `due_pay_amount` decimal(10,2) DEFAULT NULL COMMENT '应代付总金额 目前等于应收款金额',
  `actual_pay_amount` decimal(10,2) DEFAULT NULL COMMENT '实际代付总金额 实际代付信息汇总',
  `receive_status` tinyint(4) DEFAULT '0' COMMENT '0 表示未收款; 1表示部分收款;2 表示全额收款\r\n            散单汇总 2 表示全额支付\r\n            ',
  `pay_status` tinyint(4) DEFAULT NULL COMMENT '0 表示未客户代付 1 表示部分客户代付 2 表示全部代付',
  `pickup_fee` decimal(8,2) DEFAULT NULL COMMENT '提货费',
  `collect_fee` decimal(8,2) DEFAULT NULL COMMENT '到付金额',
  `partner_name` varchar(64) DEFAULT NULL COMMENT '代理名称',
  `is_internal_business` tinyint(1) DEFAULT NULL COMMENT '是否内部业务',
  `sign_man` varchar(32) DEFAULT NULL COMMENT '签收人',
  `signer_phone` varchar(16) DEFAULT NULL COMMENT '签收人联系电话',
  `id_card_no` varchar(32) DEFAULT NULL COMMENT '身份证号',
  `sign_tm` datetime DEFAULT NULL COMMENT '签收时间',
  `pickup_branch_name` varchar(64) DEFAULT NULL COMMENT '提货网点名称',
  `pickup_time` datetime DEFAULT NULL COMMENT '提货时间',
  `data_source` tinyint(4) DEFAULT NULL COMMENT '数据来源：出港0，进港1',
  `customer_id` bigint(20) DEFAULT NULL COMMENT '下单客户',
  `customer_name` varchar(32) CHARACTER SET utf8 COLLATE utf8_bin DEFAULT NULL COMMENT '客户名称',
  `origin_id` bigint(20) DEFAULT NULL COMMENT '源表分单id',
  `door_pickup_fee` decimal(8,2) DEFAULT NULL COMMENT '揽收费',
  `is_fengyi` tinyint(4) DEFAULT '0' COMMENT '是否丰羿  0：否  1：是',
  `recieve_fee` decimal(8,2) DEFAULT NULL COMMENT '寄付运费',
  `collect_other_fee` decimal(8,2) DEFAULT NULL COMMENT '到付其他',
  `saas_total_fee` decimal(8,2) DEFAULT NULL COMMENT 'SAAS运费总额',
  `is_forwarder_hawb` tinyint(4) DEFAULT NULL COMMENT '是否货代分单',
  `receive_add` decimal(8,2) DEFAULT NULL COMMENT '应收增值费用',
  PRIMARY KEY (`id`),
  UNIQUE KEY `number_code_source` (`bill_number`,`plat_company_code`,`data_source`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='结算分单表,记录结算最新的分单数据';



-- ----------------------------
-- Table structure for fin_hawb_status	分单状态表
-- ----------------------------
DROP TABLE IF EXISTS `fin_hawb_status`;
CREATE TABLE `fin_hawb_status` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `order_no` varchar(64) DEFAULT NULL COMMENT '订单号',
  `hawb_no` varchar(32) DEFAULT NULL COMMENT '分单号',
  `account_card_code` varchar(32) DEFAULT NULL COMMENT '结算帐号编码(客户编码-序列号，目前结算帐号与客户关联，可以使用客户编码；如果与合同关联，可以使用客户编码+合同编码)',
  `hawb_type` int(11) DEFAULT NULL COMMENT '分单类型（0 月结 1 散单）',
  `is_change` tinyint(4) DEFAULT NULL COMMENT '是否有数据变化(下面的任何变化都会引起这个值变化 0 没有变化;1 有变化)',
  `is_source` tinyint(4) DEFAULT NULL COMMENT '表示业务数据发生变化 0 没有变化;1 有变化 ',
  `is_so_recieve_change` tinyint(4) DEFAULT NULL COMMENT '表示散单有新的收款记录进入。（0 表示没有，1 表示有，如果有，需要进入当日散单汇总。）',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `current_bill_code` varchar(32) DEFAULT NULL COMMENT '最后一个账单号',
  `plat_company_code` varchar(32) NOT NULL,
  `data_source` tinyint(4) DEFAULT NULL COMMENT '数据来源：出港0，进港1',
  `origin_id` bigint(20) DEFAULT NULL COMMENT '源表分单id',
  `is_manual_change` tinyint(4) NOT NULL DEFAULT '0' COMMENT '是否手动调整:0-否，1-调整',
  PRIMARY KEY (`id`),
  UNIQUE KEY `fin_hawb_status_order_no_uindex` (`order_no`,`plat_company_code`),
  KEY `AK_uk_fin_hawb_status` (`hawb_no`,`plat_company_code`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='货代分单状态表：监控业务数据的情况，并且为业务一致性进行处理；主要是代收代付数据的监控';



-- ----------------------------
-- Table structure for fin_pay_flow	流水表
-- ----------------------------
DROP TABLE IF EXISTS `fin_pay_flow`;
CREATE TABLE `fin_pay_flow` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `transaction_no` varchar(32) DEFAULT NULL COMMENT '支付流水号',
  `amount` decimal(10,2) NOT NULL COMMENT '交易金额',
  `channel` tinyint(4) DEFAULT NULL COMMENT '支付渠道',
  `operation_type` tinyint(4) NOT NULL COMMENT '0-收款;1-付款',
  `status` tinyint(4) DEFAULT NULL COMMENT '0-创建;1-支付回调中;2-支付成功;3-支付失败',
  `fail_reason` varchar(256) DEFAULT NULL COMMENT '失败原因',
  `remark` varchar(256) DEFAULT NULL COMMENT '备注',
  `created_by` varchar(32) NOT NULL COMMENT '创建者',
  `creation_date` datetime NOT NULL COMMENT '创建时间',
  `updated_by` varchar(32) NOT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) NOT NULL COMMENT '是否删除',
  `plat_company_code` varchar(32) NOT NULL,
  `write_back_status` tinyint(4) DEFAULT '0' COMMENT '支付流水回写账单状态:0-未写;1-已写;',
  `write_back_time` datetime DEFAULT NULL COMMENT '支付流水回写账单时间',
  `bank_card_type` tinyint(4) DEFAULT NULL COMMENT '银行卡类型;0-个人银行卡;1-企业对公账户',
  `bank_name` varchar(32) DEFAULT NULL COMMENT '开户行名称',
  `bank_card_no` varchar(32) DEFAULT NULL COMMENT '银行卡号',
  `bank_card_holder` varchar(32) DEFAULT NULL COMMENT '开户人',
  `plat_company_name` varchar(32) DEFAULT NULL COMMENT '平台公司名称',
  `biz_user_id` varchar(32) DEFAULT NULL COMMENT '收款账号',
  `union_bank` varchar(32) DEFAULT NULL COMMENT '支付行号',
  `province` varchar(16) DEFAULT NULL COMMENT '开户行所在省名称',
  `city` varchar(16) DEFAULT NULL COMMENT '开户行所在市名称',
  `is_effect` tinyint(4) DEFAULT NULL COMMENT '是否生效 0:否；1:是',
  `is_fengyi` tinyint(4) DEFAULT '0' COMMENT '是否丰羿  0：否  1：是',
  `is_manual` tinyint(4) DEFAULT '0' COMMENT '是否手工录入 0否(线上) 1是(线下)',
  `business_order_no` varchar(64) DEFAULT NULL COMMENT '商务订单号',
  `is_receivable` tinyint(4) DEFAULT NULL COMMENT '应收代收（0-应收；1-代收）',
  PRIMARY KEY (`id`),
  UNIQUE KEY `fin_pay_flow_transaction_no_uindex` (`transaction_no`),
  KEY `fin_pay_flow_transaction_no_normal_index` (`transaction_no`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='记录每次支付操作的信息，对应支付流水的信息';



-- ----------------------------
-- Table structure for fin_pay_flow_detail	流水子表
-- ----------------------------
DROP TABLE IF EXISTS `fin_pay_flow_detail`;
CREATE TABLE `fin_pay_flow_detail` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `pay_flow_id` bigint(20) NOT NULL COMMENT '序列号',
  `bill_number` varchar(64) DEFAULT NULL COMMENT '分单号/账单编码',
  `amount` decimal(10,2) NOT NULL COMMENT '订单金额/账单金额;实际支付金额',
  `type` tinyint(4) DEFAULT NULL COMMENT '单据类型:0-月结;1-散单;',
  `allow_amount` decimal(10,2) DEFAULT NULL COMMENT '可支付金额;账单当前可支付金额',
  `created_by` varchar(32) NOT NULL COMMENT '创建者',
  `creation_date` datetime NOT NULL COMMENT '创建时间',
  `updated_by` varchar(32) NOT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) NOT NULL COMMENT '是否删除',
  `plat_company_code` varchar(32) NOT NULL,
  PRIMARY KEY (`id`),
  KEY `fin_pay_flow_detail_bill_number_index` (`bill_number`),
  KEY `fin_pay_flow_detail_pay_flow_id_index` (`pay_flow_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;



-- ----------------------------
-- Table structure for fin_pay_commission	手续费表
-- ----------------------------
DROP TABLE IF EXISTS `fin_pay_commission`;
CREATE TABLE `fin_pay_commission` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT,
  `yun_order_no` varchar(64) DEFAULT NULL COMMENT '云商通订单号',
  `type` tinyint(4) DEFAULT NULL COMMENT '订单类型:4-托管代收订单;5-单笔托管代付;',
  `amount` decimal(18,7) DEFAULT NULL COMMENT '交易金额，单位：元',
  `handling_fee` decimal(18,7) DEFAULT NULL COMMENT '手续费金额，单位：元',
  `pay_time` datetime DEFAULT NULL COMMENT '交易时间',
  `transaction_no` varchar(64) DEFAULT NULL COMMENT '支付单号',
  `channel_fee` decimal(18,7) DEFAULT NULL COMMENT '渠道金额，单位：元',
  `channel_no` varchar(64) DEFAULT NULL COMMENT '渠道流水号',
  `channel_handling_fee` decimal(18,7) DEFAULT NULL COMMENT '渠道手续费，单位：元',
  `yun_handling_fee` decimal(18,7) DEFAULT NULL COMMENT '云商通手续费，单位：元',
  `name` varchar(64) DEFAULT NULL COMMENT '供应商名称/客户名称',
  `created_by` varchar(32) NOT NULL COMMENT '创建者',
  `creation_date` datetime NOT NULL COMMENT '创建时间',
  `updated_by` varchar(32) NOT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) NOT NULL COMMENT '是否删除',
  `status` tinyint(4) DEFAULT '0' COMMENT '状态;0-未确认;1-已确认.',
  PRIMARY KEY (`id`),
  KEY `fin_pay_commission_transaction_no_normal_index` (`transaction_no`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='支付手续费';


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------



-- ----------------------------
-- Table structure for fin_invoice_base_info	发票基础信息维护
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_base_info`;
CREATE TABLE `fin_invoice_base_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `customer_code` varchar(32) NOT NULL COMMENT '客户编码',
  `customer_name` varchar(32) NOT NULL COMMENT '客户名称',
  `plat_company_code` varchar(32) DEFAULT NULL COMMENT '平台公司编码',
  `company_name` varchar(64) NOT NULL COMMENT '单位名称',
  `memo` varchar(256) DEFAULT NULL COMMENT '备注',
  `buyer_address` varchar(256) NOT NULL COMMENT '开票地址',
  `buyer_phone` varchar(16) NOT NULL COMMENT '开票电话',
  `buyer_bank` varchar(64) NOT NULL COMMENT '开票银行',
  `buyer_bank_account` varchar(32) NOT NULL COMMENT '开票银行账号',
  `receiver_address` varchar(256) NOT NULL COMMENT '收票人地址',
  `receiver` varchar(32) NOT NULL COMMENT '收票人姓名',
  `receiver_mobile` varchar(32) NOT NULL COMMENT '收票人手机',
  `receiver_mail` varchar(32) NOT NULL COMMENT '收票人邮箱',
  `is_default_look_up` tinyint(4) DEFAULT NULL COMMENT '是否默认抬头(0-否；1-是)',
  `taxpayer_identification_number` varchar(32) NOT NULL COMMENT '纳税人识别号',
  `created_by` varchar(32) NOT NULL COMMENT '创建者',
  `creation_date` datetime NOT NULL COMMENT '创建时间',
  `updated_by` varchar(32) NOT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `info_type` tinyint(4) DEFAULT NULL COMMENT '信息类型(0-客户向平台；1-货代代客户向平台；2-平台向货代)',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='开票基础信息维护表';


-- ----------------------------
-- Table structure for fin_invoice	发票表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice`;
CREATE TABLE `fin_invoice` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `invoice_send_id` bigint(20) DEFAULT NULL COMMENT '序列号',
  `invoice_apply_id` bigint(20) NOT NULL COMMENT '序列号',
  `apply_code` varchar(64) NOT NULL COMMENT '发票申请编码',
  `apply_type` tinyint(4) NOT NULL COMMENT '申请类型(0-客户向平台；1-货代代客户向平台；2-平台向货代)',
  `sp_code` varchar(64) NOT NULL DEFAULT '' COMMENT '供应商编码',
  `invoice_type` tinyint(4) NOT NULL COMMENT '发票类型：1-增值税普通发票 2-增值税专用发票 3-电子普通发票',
  `invoice_code` varchar(64) DEFAULT NULL COMMENT '发票代码',
  `invoice_no` varchar(64) DEFAULT NULL COMMENT '发票号',
  `pc_code` varchar(64) DEFAULT NULL COMMENT '机器编码',
  `invoice_date` datetime DEFAULT NULL COMMENT '开票日期',
  `ele_inv_pdf_url` varchar(256) DEFAULT NULL COMMENT '电子发票pdf文件url',
  `ele_inv_pic_url` varchar(64) DEFAULT NULL COMMENT '电子发票图片url',
  `invoice_file_name` varchar(64) DEFAULT NULL COMMENT '专票文件名',
  `customer_code` varchar(32) DEFAULT NULL COMMENT '客户编码',
  `customer_name` varchar(32) DEFAULT NULL COMMENT '客户名称',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) DEFAULT '1' COMMENT '版本号',
  PRIMARY KEY (`id`),
  KEY `index_invoice_send_id` (`invoice_send_id`) USING BTREE,
  KEY `index_invoice_apply_id` (`invoice_apply_id`) USING BTREE,
  KEY `index_apply_code` (`apply_code`) USING BTREE,
  KEY `index_sp_code` (`sp_code`),
  KEY `index_invoice_date` (`invoice_date`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='发票信息表';


-- ----------------------------
-- Table structure for fin_invoice_detail	发票明细表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_detail`;
CREATE TABLE `fin_invoice_detail` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `invoice_id` bigint(20) NOT NULL COMMENT '序列号',
  `fee_type` varchar(16) NOT NULL COMMENT '费用编码',
  `invoice_amount` decimal(8,0) NOT NULL COMMENT '开票金额',
  `rate` decimal(8,2) NOT NULL COMMENT '税率',
  `tax` decimal(8,2) NOT NULL COMMENT '税额',
  `no_tax_amount` decimal(8,2) NOT NULL COMMENT '不含税金额',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) DEFAULT '1' COMMENT '版本号',
  PRIMARY KEY (`id`),
  KEY `index_invoice_id` (`invoice_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=35 DEFAULT CHARSET=utf8 COMMENT='发票税率信息表';



-- ----------------------------
-- Table structure for fin_invoice_apply	发票申请
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_apply`;
CREATE TABLE `fin_invoice_apply` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `apply_code` varchar(64) NOT NULL COMMENT '发票申请编码',
  `apply_type` tinyint(4) NOT NULL COMMENT '申请类型(0-客户向平台；1-货代代客户向平台；2-平台向货代)',
  `invoice_type` tinyint(4) DEFAULT NULL COMMENT '发票类型：1-增值税普通发票 2-增值税专用发票; 3-电子普通发票;',
  `biz_type` tinyint(4) DEFAULT NULL COMMENT '业务类型',
  `biz_codes` varchar(1024) DEFAULT NULL COMMENT '业务单号',
  `apply_date` datetime DEFAULT NULL COMMENT '发票申请日期',
  `invoice_title` varchar(64) DEFAULT NULL COMMENT '发票抬头',
  `company_name` varchar(64) DEFAULT NULL COMMENT '单位名称',
  `buyer_name` varchar(64) DEFAULT NULL COMMENT '开票人名称',
  `buyer_address` varchar(256) DEFAULT NULL COMMENT '开票地址',
  `buyer_phone` varchar(16) DEFAULT NULL COMMENT '开票电话',
  `buyer_bank` varchar(64) DEFAULT NULL COMMENT '开票银行',
  `buyer_bank_account` varchar(32) DEFAULT NULL COMMENT '开票银行账号',
  `receiver_address` varchar(256) DEFAULT NULL COMMENT '收票人地址',
  `receiver` varchar(32) DEFAULT NULL COMMENT '收票人姓名',
  `receiver_mobile` varchar(32) DEFAULT NULL COMMENT '收票人手机',
  `receiver_mail` varchar(32) DEFAULT NULL COMMENT '收票人邮箱',
  `taxpayer_identification_number` varchar(32) DEFAULT NULL COMMENT '纳税人识别号',
  `invoice_amount` decimal(10,2) NOT NULL COMMENT '开票金额',
  `customer_code` varchar(32) DEFAULT NULL COMMENT '客户编码',
  `customer_name` varchar(32) DEFAULT NULL COMMENT '客户名称',
  `sp_name` varchar(64) DEFAULT NULL COMMENT '供应商名称',
  `sp_code` varchar(64) NOT NULL DEFAULT '' COMMENT '供应商编码',
  `remark` varchar(264) DEFAULT NULL COMMENT '备注',
  `reject_remark` varchar(256) DEFAULT NULL COMMENT '驳回备注',
  `invoice_apply_status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '申请人开票状态(0-未开票、1-已开票、2-开票中、3-已驳回、4-已寄送、5-已撤销、6-电子发票申请失败)',
  `approve_status` tinyint(4) NOT NULL DEFAULT '0' COMMENT '审核状态(0-未开票、1-已开票、2-开票中、3-已驳回、4-已寄送、5-已撤销、6-申请失败)',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) NOT NULL DEFAULT '1' COMMENT '版本号',
  `send_email` tinyint(4) DEFAULT NULL COMMENT '是否发送邮箱:0-不发;1-发',
  PRIMARY KEY (`id`),
  UNIQUE KEY `index_apply_code` (`apply_code`) USING BTREE,
  KEY `index_sp_code` (`sp_code`),
  KEY `index_apply_date` (`apply_date`)
) ENGINE=InnoDB AUTO_INCREMENT=13327 DEFAULT CHARSET=utf8 COMMENT='发票申请表';


-- ----------------------------
-- Table structure for fin_invoice_apply_detail	发票申请明细
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_apply_detail`;
CREATE TABLE `fin_invoice_apply_detail` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `invoice_apply_id` bigint(20) NOT NULL COMMENT '序列号',
  `fee_type` varchar(16) NOT NULL COMMENT '费用编码',
  `invoice_amount` decimal(8,0) NOT NULL COMMENT '开票金额',
  `rate` decimal(8,2) NOT NULL COMMENT '税率',
  `tax` decimal(8,2) NOT NULL COMMENT '税金',
  `no_tax_amount` decimal(8,2) NOT NULL COMMENT '不含税金额',
  `discount` decimal(6,2) DEFAULT NULL COMMENT '折扣',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) DEFAULT '1' COMMENT '版本号',
  PRIMARY KEY (`id`),
  KEY `index_invoice_apply_id` (`invoice_apply_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=206 DEFAULT CHARSET=utf8 COMMENT='发票申请明细表';



-- ----------------------------
-- Table structure for fin_invoice_apply_biz_data	发票业务数据表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_apply_biz_data`;
CREATE TABLE `fin_invoice_apply_biz_data` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `plat_company_code` varchar(32) DEFAULT NULL COMMENT '平台公司编码',
  `plat_company_name` varchar(32) DEFAULT NULL COMMENT '平台公司名称',
  `biz_id` bigint(20) NOT NULL COMMENT '业务Id',
  `biz_code` varchar(64) NOT NULL COMMENT '业务单据号',
  `biz_type` tinyint(4) NOT NULL COMMENT '业务类型;0-月结账单;1-散单;2-分单汇总;',
  `invoice_status` tinyint(4) DEFAULT '0' COMMENT '业务单据发票状态:0-可开票;1-开票中;2-已开票',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `processing_amount` decimal(8,2) DEFAULT NULL COMMENT '开票中的金额',
  `processed_amount` decimal(8,2) DEFAULT NULL COMMENT '已开票的金额',
  `amount` decimal(8,2) NOT NULL COMMENT '单据总金额',
  `version` int(11) NOT NULL DEFAULT '1' COMMENT '版本号',
  `apply_type` tinyint(4) NOT NULL COMMENT '申请类型 这里是数据流向 (0-客户向平台；1-货代代客户向平台；2-平台向货代)',
  PRIMARY KEY (`id`),
  UNIQUE KEY `biz_data_unique_index` (`biz_id`,`plat_company_code`,`biz_type`,`apply_type`),
  KEY `index_biz_code` (`biz_code`) USING BTREE,
  KEY `index_biz_id` (`biz_id`)
) ENGINE=InnoDB AUTO_INCREMENT=59 DEFAULT CHARSET=utf8 COMMENT='发票申请业务数据表';



-- ----------------------------
-- Table structure for fin_invoice_apply_biz_detail_data	发票业务数据明细表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_apply_biz_detail_data`;
CREATE TABLE `fin_invoice_apply_biz_detail_data` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `main_id` bigint(20) NOT NULL COMMENT '序列号',
  `plat_company_code` varchar(32) DEFAULT NULL COMMENT '平台公司编码',
  `rate` decimal(8,2) DEFAULT NULL COMMENT '税率',
  `amount` decimal(8,2) DEFAULT NULL COMMENT '总金额',
  `description` varchar(256) DEFAULT NULL COMMENT '描述',
  `processing_amount` decimal(8,2) DEFAULT NULL COMMENT '开票中的金额',
  `processed_amount` decimal(8,2) DEFAULT NULL COMMENT '已开票的金额',
  `version` int(11) NOT NULL DEFAULT '1' COMMENT '版本号',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  PRIMARY KEY (`id`),
  KEY `index_main_id` (`main_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=181 DEFAULT CHARSET=utf8 COMMENT='发票申请业务数据费用项表';


-- ----------------------------
-- Table structure for fin_invoice_apply_biz_data_association	发票申请与业务数据关联表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_apply_biz_data_association`;
CREATE TABLE `fin_invoice_apply_biz_data_association` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `biz_data_id` bigint(20) NOT NULL COMMENT '序列号',
  `invoice_apply_id` bigint(20) NOT NULL COMMENT '序列号',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) NOT NULL DEFAULT '1' COMMENT '版本号',
  `rate` decimal(8,2) NOT NULL COMMENT '税率',
  `amount` decimal(8,2) NOT NULL COMMENT '总金额',
  `description` varchar(256) DEFAULT NULL COMMENT '描述',
  PRIMARY KEY (`id`),
  KEY `index_biz_data_id` (`biz_data_id`) USING BTREE,
  KEY `index_invoice_apply_id` (`invoice_apply_id`) USING BTREE
) ENGINE=InnoDB AUTO_INCREMENT=218 DEFAULT CHARSET=utf8 COMMENT='发票申请表及发票申请业务数据表的关联表';




-- ----------------------------
-- Table structure for fin_invoice_send	发票寄送表
-- ----------------------------
DROP TABLE IF EXISTS `fin_invoice_send`;
CREATE TABLE `fin_invoice_send` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT '序列号',
  `send_date` datetime DEFAULT NULL COMMENT '寄送日期',
  `send_status` tinyint(4) DEFAULT NULL COMMENT '寄送状态 0 未寄送 1 已寄送',
  `express_company` varchar(64) DEFAULT NULL COMMENT '快递公司名称',
  `express_code` varchar(32) DEFAULT NULL COMMENT '快递单号',
  `receiver_address` varchar(256) DEFAULT NULL COMMENT '收票人地址',
  `receiver` varchar(32) DEFAULT NULL COMMENT '收票人姓名',
  `receiver_mobile` varchar(32) DEFAULT NULL COMMENT '收票人手机',
  `receiver_mail` varchar(32) DEFAULT NULL COMMENT '收票人邮箱',
  `memo` varchar(256) DEFAULT NULL COMMENT '备注',
  `created_by` varchar(32) DEFAULT NULL COMMENT '创建者',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(32) DEFAULT NULL COMMENT '修改者',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` tinyint(4) DEFAULT NULL COMMENT '是否删除',
  `version` int(11) DEFAULT '1' COMMENT '版本号',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=26 DEFAULT CHARSET=utf8 COMMENT='发票寄送信息表';








-- ----------------------------
-- Table structure for bil_invoice
-- ----------------------------
DROP TABLE IF EXISTS `bil_invoice`;
CREATE TABLE `bil_invoice` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `apply_code` varchar(64) DEFAULT NULL COMMENT '发票申请编号',
  `invoice_code` varchar(64) DEFAULT NULL COMMENT '发票代码',
  `invoice_no` varchar(64) DEFAULT NULL COMMENT '发票号',
  `date` date DEFAULT NULL COMMENT '开票日期',
  `amount` decimal(18,7) DEFAULT NULL COMMENT '不含税金额',
  `tax` decimal(18,7) DEFAULT NULL COMMENT '税额',
  `rate` decimal(18,7) DEFAULT NULL COMMENT '税率',
  `invoice_amount` decimal(18,7) DEFAULT NULL COMMENT '发票金额',
  `tax_flag` int(1) DEFAULT NULL COMMENT '税务类型 专票/普票',
  `invoice_send_id` bigint(20) DEFAULT NULL COMMENT '对应哪个寄送记录(bil_invoice_send的id)',
  `status` int(1) DEFAULT '0' COMMENT '0:有效 1:废弃',
  `created_by` varchar(64) DEFAULT NULL COMMENT '创建人',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(64) DEFAULT NULL COMMENT '修改人',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` int(4) DEFAULT NULL COMMENT '删除标志',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='发票信息表';

-- ----------------------------
-- Table structure for bil_invoice_apply
-- ----------------------------
DROP TABLE IF EXISTS `bil_invoice_apply`;
CREATE TABLE `bil_invoice_apply` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `apply_code` varchar(64) DEFAULT NULL COMMENT '发票申请单编号  （业务主键）',
  `apply_date` date DEFAULT NULL COMMENT '发票申请日期',
  `customer_name` varchar(255) DEFAULT NULL COMMENT '客户名称',
  `customer_code` varchar(64) DEFAULT NULL COMMENT '客户编码',
  `bill_code` varchar(64) DEFAULT NULL COMMENT '账单编号 是否是账单表的ID ',
  `bill_id` bigint(20) DEFAULT NULL COMMENT '账单表ID',
  `amount` decimal(18,7) NOT NULL COMMENT '开票金额 月结：账单实收金额 散单：',
  `bill_type` tinyint(1) NOT NULL COMMENT '账期类型: 0-散单; 1-月结;',
  `status` tinyint(1) NOT NULL DEFAULT '0' COMMENT '开票状态：0：未开票; 1:开票; 2:已寄送;',
  `order_ids` varchar(255) DEFAULT NULL COMMENT '散单发票关联的订单id列表',
  `created_by` varchar(64) DEFAULT NULL COMMENT '创建人',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(64) DEFAULT NULL COMMENT '修改人',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` int(4) DEFAULT NULL COMMENT '删除标志',
  `buyer_name` varchar(255) DEFAULT NULL COMMENT '购方名称',
  `buyer_tax_no` varchar(255) DEFAULT NULL COMMENT '购方税号',
  `buyer_address` varchar(255) DEFAULT NULL COMMENT '购方地址',
  `buyer_mobile` varchar(100) DEFAULT NULL COMMENT '购方电话',
  `buyer_bank` varchar(100) DEFAULT NULL COMMENT '购方银行',
  `buyer_bank_account` varchar(64) DEFAULT NULL COMMENT '购方银行账号',
  `receiver` varchar(255) DEFAULT NULL COMMENT '收票人',
  `receiver_mobile` varchar(255) DEFAULT NULL COMMENT '收票人联系方式',
  `receiver_address` varchar(255) DEFAULT NULL COMMENT '收票人地址',
  `invoice_type` int(2) DEFAULT NULL COMMENT '发票类型;0-普票;1-专票;',
  `order_nos` varchar(255) DEFAULT NULL COMMENT '订单号列表',
  `zyd_flag` tinyint(3) unsigned DEFAULT NULL COMMENT '为1则是非平台产品',
  `product_code` varchar(64) DEFAULT NULL COMMENT '产品编码',
  `product_family` int(11) DEFAULT '0' COMMENT '产品家庭.0-快运;1-空舱;',
  PRIMARY KEY (`id`),
  UNIQUE KEY `idx_bil_invoice_apply_1` (`apply_code`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='发票申请表';

-- ----------------------------
-- Table structure for bil_invoice_apply_detail
-- ----------------------------
DROP TABLE IF EXISTS `bil_invoice_apply_detail`;
CREATE TABLE `bil_invoice_apply_detail` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `amount` decimal(18,7) DEFAULT NULL COMMENT '不含税金额',
  `tax` decimal(18,7) DEFAULT NULL COMMENT '税金',
  `discount` decimal(18,7) DEFAULT NULL COMMENT '折扣 0',
  `ar_amount` decimal(18,7) DEFAULT NULL COMMENT '实收金额',
  `main_id` bigint(20) DEFAULT NULL COMMENT '发票申请单id(bth_mth_invoice_apply的id/bth_so_invoice_apply的id)',
  `is_so` int(1) DEFAULT NULL COMMENT '是否散单 0 ：是 1：否',
  `invoice_type` int(1) DEFAULT NULL COMMENT '发票类型; 0-普票; 1-专票;',
  `fee_type` varchar(255) DEFAULT NULL COMMENT '费用类型 (目前仅运输服务费)',
  `rate` decimal(18,7) DEFAULT NULL COMMENT '税率',
  `created_by` varchar(64) DEFAULT NULL COMMENT '创建人',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(64) DEFAULT NULL COMMENT '修改人',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` int(4) DEFAULT '0' COMMENT '删除标志',
  `remark` varchar(512) DEFAULT NULL COMMENT '备注',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='发票申请明细表 ';

-- ----------------------------
-- Table structure for bil_invoice_send
-- ----------------------------
DROP TABLE IF EXISTS `bil_invoice_send`;
CREATE TABLE `bil_invoice_send` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `customer_name` varchar(255) NOT NULL COMMENT '客户名称',
  `customer_code` varchar(64) DEFAULT NULL COMMENT '客户编码',
  `sender` varchar(255) DEFAULT NULL COMMENT '寄件人',
  `sender_contract` varchar(255) DEFAULT NULL COMMENT '寄件人联系方式',
  `send_date` date DEFAULT NULL COMMENT '寄送日期',
  `status` int(1) DEFAULT '1' COMMENT '寄送状态: 0:未寄送; 1:已寄送;',
  `reciever` varchar(255) DEFAULT NULL COMMENT '接收人',
  `reciever_phone` varchar(255) DEFAULT NULL COMMENT '收件人固定电话',
  `reciever_mobile` varchar(255) DEFAULT NULL COMMENT '收件人移动电话',
  `reciever_address` varchar(255) DEFAULT NULL COMMENT '收件人地址',
  `send_flag` int(1) DEFAULT NULL COMMENT '送件方式:自送 Y  寄送 N',
  `express_company` varchar(255) DEFAULT NULL,
  `express_code` varchar(255) DEFAULT NULL COMMENT '快递单号',
  `note` varchar(255) DEFAULT NULL COMMENT '备注',
  `created_by` varchar(64) DEFAULT NULL COMMENT '创建人',
  `creation_date` datetime DEFAULT NULL COMMENT '创建时间',
  `updated_by` varchar(64) DEFAULT NULL COMMENT '修改人',
  `updation_date` datetime DEFAULT NULL COMMENT '修改时间',
  `deleted` int(4) DEFAULT NULL COMMENT '删除标志',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8 COMMENT='发票寄送信息';




======================================================================================================================================================================================================================================




package com.wukong.atp.billing.cp.server.common.util;

import javax.servlet.http.HttpServletRequest;

import org.apache.commons.codec.binary.Base64;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import com.alibaba.fastjson.JSON;
import com.alibaba.fastjson.JSONObject;
import com.alibaba.fastjson.serializer.SerializerFeature;
import com.wukong.atp.billing.cp.server.common.util.MsgBean.MsgCode;

public class BaseController {
	
	Logger logger = LoggerFactory.getLogger(BaseController.class);
	
	private static final String COMMON_EXCEPTION_MSG = "系统正在维护中,请稍后再试";
	
	
	/**
	 * 获取前端传给后台的参数
	 * @param 	request	http请求
	 * @return  RequestParamModel 参数对象
	 * */
	protected RequestParamModel getParameter(HttpServletRequest request) {
		return (RequestParamModel) request.getAttribute("param");
	}
	
	
	/**
	 * 获取前端传给后台的参数
	 * @param request http请求
	 * @param clazz 类定义
	 * @return Q 类实体
	 * */
	protected <Q extends BaseParam> Q getEntityParameter(HttpServletRequest request, Class<Q> clazz){
		JSONObject entity = this.getParameter(request).getEntity();
		Q obj;
		if (entity == null) {
			String msg;
			try {
				obj = clazz.newInstance();
			} catch (Exception e) {
				msg = COMMON_EXCEPTION_MSG;
				throw new IllegalArgumentException(msg);
			} 
		} else {
			obj = JSON.parseObject(entity.toString(), clazz);
		}
		return obj;
	}
	
	
	
	
	
	
	/**
	 * 封装返回结果
	 * @param code 消息编码
	 * @param detail 消息内容
	 * @return ResponseResultModel 结果参数对象
	 */
	protected ResponseResultModel encapsulateResult(MsgCode code, String detail) {
		return encapsulateResult(null, code, detail);
	}
	
	
	/**
	 * 封装返回结果,日期格式化
	 * */
	protected <T> ResponseResultModel encapsulateResult(T result, MsgCode code, String detail) {
		MsgBean<T> msgBean = new MsgBean<>();
		msgBean.setEntity(result);
		msgBean.setCode(code).setDetail(detail);
		// 将对象转换为json字符串
		String originResultString = JSON.toJSONStringWithDateFormat(msgBean, "yyyy-MM-dd HH:mm:ss", SerializerFeature.WriteMapNullValue);
		logger.info("============返回原始数据：" + originResultString);
		ResponseResultModel responseResultModel = new ResponseResultModel();
		// Base64封装返回
		responseResultModel.setResult(Base64.encodeBase64String(originResultString.getBytes()));
		return responseResultModel;
	}
	
	/**
	 * 封装返回结果,不使用日期格式化
	 */
	protected <T> ResponseResultModel encapsulateResultWithoutDateFormate(T result, MsgCode code, String detail) {
		MsgBean<T> msgBean = new MsgBean<>();
		msgBean.setCode(code).setDetail(detail);
		ResponseResultModel responseResultModel = new ResponseResultModel();
		// 将对象转换为json字符串
		String originResultString = JSON.toJSONString(msgBean, SerializerFeature.WriteMapNullValue);
		logger.info("============返回原始数据：" + originResultString);
		// Base64封装返回
		responseResultModel.setResult(Base64.encodeBase64String(originResultString.getBytes()));
		return responseResultModel;
	}
	
	/**
	 * 封装返回结果
	 * @param msgBean 封装实体
	 * @return ResponseResultModel 结果参数对象
	 */
	protected <T> ResponseResultModel encapsulateResult(MsgBean<T> msgBean) {
		ResponseResultModel responseResultModel = new ResponseResultModel();
		// 将对象转换为json字符串
		String originResultString = JSON.toJSONStringWithDateFormat(msgBean, "yyyy-MM-dd HH:mm:ss", SerializerFeature.WriteMapNullValue);
		logger.info("============返回原始数据：" + originResultString);
		// Base64封装返回
		responseResultModel.setResult(Base64.encodeBase64String(originResultString.getBytes()));
		return responseResultModel;
	}
	
	
	
	

	/**
	 * 获取用户信息
	 * @return UserInfo 用户信息
	 */
	protected UserInfo getUserInfo() {
		Context context = ContextHolder.get();
		return context.getUserInfo();
	}
	
	/**
	 * 获取用户信息Id
	 * @return userId 用户信息id
	 */
	protected String getUserId() {
		Context context = ContextHolder.get();
		return context.getUserInfo().getUserId();
	}
	

}



=====================================================================================================================================================================================================================================



package com.wukong.atp.billing.cp.server.common.util;

import java.io.Serializable;
import java.lang.reflect.Field;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;


/**
 * @Description BaseParam
 * @author: 80003253    
 * Create at: 2019年6月17日 上午10:00:04
 */
public class BaseParam implements Serializable{

	private static final long serialVersionUID = 30541166914834587L;
	
	private static final Logger logger = LoggerFactory.getLogger(BaseParam.class);
	
	
	/**
	 * 获取参数字段键值对 key：字段名称 value：字段值
	 * @param name
	 * @return KeyValuePair
	 */
	private KeyValuePair<String, Object> getFild(String name){
		try {
			KeyValuePair<String, Object> KeyValuePair = getDeclaredField(this, name);
			if (KeyValuePair == null) {
				throw new IllegalArgumentException("参数字段解析异常");
			}
			return getDeclaredField(this, name);
		} catch (Exception e) {
			throw new IllegalArgumentException("参数字段解析异常");
		}
	}
	
	
	
	private KeyValuePair<String, Object> getDeclaredField(Object object, String fildName){
		Class<?> clazz = object.getClass();
		if (clazz == Class.class) {
			return null;
		}
		return getDeclaredField(clazz, fildName);
	}
	
	
	private KeyValuePair<String, Object> getDeclaredField(Class<?> clazz, String fildName){
		if (clazz == null) {
			return null;
		}
		try {
			KeyValuePair<String, Object> keyValuePair = new KeyValuePair<>();
			keyValuePair.setKey(fildName);
			Field field = clazz.getDeclaredField(fildName);
			field.setAccessible(true);
			keyValuePair.setValue(field.get(this));
			return keyValuePair;
			
		} catch (NoSuchFieldException e) {
			return getDeclaredField(clazz.getSuperclass(), fildName);
		} catch (Exception e) {
			logger.info("获取参数字段名称出现系统异常", e);
		}
		return null;
	}
	
	

}


=====================================================================================================================================================================================================================================


package com.wukong.atp.billing.cp.server.common.util;
/**
 * @Description Context 上下文定义 
 * @author: 80003253    
 * Create at: 2019年6月17日 下午3:59:51
 */

import java.util.concurrent.ConcurrentHashMap;

public class Context {

	private UserInfo userInfo;
	
	private ConcurrentHashMap<String, Object> globalVariableMap = new ConcurrentHashMap<>();

	
	
	
	public UserInfo getUserInfo() {
		return userInfo;
	}

	public void setUserInfo(UserInfo userInfo) {
		this.userInfo = userInfo;
	}
	
	

	public ConcurrentHashMap<String, Object> getGlobalVariableMap() {
		return globalVariableMap;
	}

	public void setGlobalVariableMap(ConcurrentHashMap<String, Object> globalVariableMap) {
		this.globalVariableMap = globalVariableMap;
	}
	
	
	/**
	 * ConcurrentHashMap添加键值
	 * */
	public void addGlobalVariable(String key, Object value) {
		globalVariableMap.put(key, value);
	}

	/**
	 * 传入键，获取值
	 * */
	public Object getGlobalVariable(String key) {
		return globalVariableMap.get(key);
	}
	
	
	
}


=====================================================================================================================================================================================================================================

package com.wukong.atp.billing.cp.server.common.util;


/**
 * @Description ContextHolder
 * @author: 80003253    
 * Create at: 2019年6月17日 下午4:07:42
 */
public class ContextHolder {
	
	private static  final ThreadLocal<Context> CURRENTLOCALCONTEXT = new InheritableThreadLocal<>();
	
	
	/**
	 * 获取上下文
	 * */
	public static Context get() {
		if (null == CURRENTLOCALCONTEXT.get()) {
			Context context = new Context();
			CURRENTLOCALCONTEXT.set(context);
		}
		return CURRENTLOCALCONTEXT.get();
	}
	
	/**
	 * 设置上下文
	 * */
	public static void set(Context context) {
		CURRENTLOCALCONTEXT.set(context);
	}
	
	/**
	 * 消除 复原 上下文
	 * */
	public static void unset() {
		CURRENTLOCALCONTEXT.remove();
	}
	
	
	
	public static void addGlobalVariable(String key, Object value) {
		// 获取上下文
		Context context = get();
		if (context == null) {
			set(new Context());
			context = get();
		}
		// ConcurrentHashMap添加键值
		context.addGlobalVariable(key, value);
	}
	
	
	public Object getGlobalVariable(String key) {
		Object result = null;
		Context context = get();
		if (context != null) {
			result = context.getGlobalVariable(key);
		}
		return result;
	}
	
	

}


=====================================================================================================================================================================================================================================



 /**   Copyright (c) 2017-2050 sf-express Co.,Ltd
 *    Package: com.wukong.atp.billing.cp.server.common.util
 *    Filename: GeneralBusinessException.java
 *    Copyright: Copyright (c) 2017-2050
 *    Company: sf-express Co.,Ltd
 *    @author: 80003253    
 *    @version: 1.0.0
 *    Create at: 2019年6月17日 上午11:28:45
 */
package com.wukong.atp.billing.cp.server.common.util;

import lombok.Getter;
import lombok.Setter;

/**
 * @Description GeneralBusinessException
 * @author: 80003253    
 * Create at: 2019年6月17日 上午11:28:45
 */
@Getter
@Setter
public class GeneralBusinessException extends RuntimeException{

	private static final long serialVersionUID = 2382109871398312377L;
	
	/**
	 * 异常码，可自定义，通常为约定值，便于后续业务处理
	 */
	private int code;

	/**
	 * 默认无参构造函数
	 */
	public GeneralBusinessException() {
		super();
	}
	
	/**
	 * 构造函数
	 * @param message 自定义异常信息
	 */
	public GeneralBusinessException(String message) {
		super(message);
	}
	
	/**
	 * 构造函数
	 * @param message 自定义异常信息
	 * @param code 异常码
	 * */
	public GeneralBusinessException(String message, int code) {
		super(message);
		this.code = code;
	}
	
}


=====================================================================================================================================================================================================================================


package com.wukong.atp.billing.cp.server.common.util;

import java.io.Serializable;

import lombok.Data;

/**
 * @Description 键值对实体类
 * @author: 80003253    
 * Create at: 2019年6月17日 下午4:39:42
 */
@Data
public class KeyValuePair<K, V> implements Serializable{
	
	private static final long serialVersionUID = -7274570875505083866L;
	
	private K key;
	
	private V value;
	
	
	/**
	 * 无参构造函数
	 * */
	public KeyValuePair() {
		
	}
	
	/**
	 * 带参构造函数
	 * @param key   键
	 * @param value 值
	 * */
	public KeyValuePair(K key, V value) {
		this.key = key;
		this.value = value;
	}
	

}


=====================================================================================================================================================================================================================================


 /**   Copyright (c) 2017-2050 sf-express Co.,Ltd
 *    Package: com.wukong.atp.billing.cp.server.common.util
 *    Filename: MsgBean.java
 *    Copyright: Copyright (c) 2017-2050
 *    Company: sf-express Co.,Ltd
 *    @author: 80003253    
 *    @version: 1.0.0
 *    Create at: 2019年6月17日 上午11:23:50
 */
package com.wukong.atp.billing.cp.server.common.util;

import java.io.Serializable;

/**
 * @Description MsgBean
 * @author: 80003253    
 * Create at: 2019年6月17日 上午11:23:50
 */
public class MsgBean<T> implements Serializable{

	private static final long serialVersionUID = -1601954185978399143L;

	// 消息编码
	private MsgCode code;
	
	// 消息详情
	private String detail;
	
	// 消息结果
	private T entity;
	
	// 业务异常
	private GeneralBusinessException generalBusinessException;
	
	/**
	 * MsgBean 构造函数1
	 * */
	public MsgBean() {
		
	}
	
	/**
	 * MsgBean 构造函数2	
	 * @param code	异常代码
	 * */
	public MsgBean(MsgCode code) {
		this.code = code;
	}

	/**
	 * MsgBean 构造函数3
	 * @param code	 异常代码
	 * @param detail 异常详情字符串
	 * */
	public MsgBean(MsgCode code, String detail) {
		this.code = code;
		this.detail = detail;
	}
	
	
	
	/**
	 * 消息编码枚举
	 * @Description Code枚举
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



	
	

	/**
	 * code getter & setter
	 * */ 
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

	
	
	/**
	 * detail getter & setter
	 * */                                                 
	public String getDetail() {
		return detail;
	}

	public MsgBean<T> setDetail(String detail) {
		this.detail = detail;
		return this;
	}
	

	
	/**
	 * entity getter & setter
	 * */
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

=====================================================================================================================================================================================================================================


package com.wukong.atp.billing.cp.server.common.util;
/**
 * @Description RequestParamModel
 * @author: 80003253    
 * Create at: 2019年6月17日 上午9:23:22
 */

import com.alibaba.fastjson.JSONObject;

import lombok.Data;

@Data
public class RequestParamModel {

	/**
	 * 统一封装请求参数
	 * */
	private JSONObject entity;
	
}


=====================================================================================================================================================================================================================================


package com.wukong.atp.billing.cp.server.common.util;

import java.io.Serializable;

import lombok.Data;

/**
 * @Description ResponseResultModel
 * @author: 80003253    
 * Create at: 2019年6月17日 上午10:48:33
 */
@Data
public class ResponseResultModel implements Serializable{

	private static final long serialVersionUID = -7621937518207837719L;

	private String result;
	
}

=====================================================================================================================================================================================================================================

package com.wukong.atp.billing.cp.server.common.util;

import java.io.Serializable;

import lombok.Getter;
import lombok.Setter;

/**
 * @Description UserInfo
 * @author: 80003253    
 * Create at: 2019年6月17日 下午3:57:36
 */
@Getter
@Setter
public class UserInfo implements Serializable{

	private static final long serialVersionUID = 4399716180150908094L;
	
	/**
	 * 用户ID
	 */
	private String userId;

	/**
	 * 用户名
	 */
	private String userName;

	/**
	 * 账号
	 */
	private String account;
	
	/**
	 * 用户所属公司编码
	 */
	private String companyCode;
	

}


=====================================================================================================================================================================================================================================






