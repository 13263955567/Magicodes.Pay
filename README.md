# Magicodes.Pay
ͳһ֧���⣬֧��֧������΢��֧���Լ�֧��������֧���⡣ʹ�ñ�׼���д������ͳһ֧���ص��ķ�װ��
## ����

���������ο��˴���:https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Startup/PayStartup.cs

<img src="res/1.png">
<img src="res/2.png">

֧����ش�����Բο�:
https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Services/PayAppService.cs

����ο�:
<img src="res/10.png">
<img src="res/11.png">
<img src="res/12.png">

## ΢��֧��

            if (WeChatPayApi == null)
            {
                throw new UserFriendlyException("֧��δ���ţ�����ϵ����Ա��");
            }
            var appPayInput = new WeChat.Pay.Dto.AppPayInput
            {
                Body = input.Body,
                OutTradeNo = input.OutTradeNo,
                Attach = input.CustomData,
                TotalFee = input.TotalAmount,
                SpbillCreateIp = _clientInfoProvider?.ClientIpAddress
            };
            try
            {
                var appPayOutput = WeChatPayApi.AppPay(appPayInput);
                return Task.FromResult(appPayOutput);
            }
            catch (Exception ex)
            {
                throw new UserFriendlyException(ex.Message);
            }

## ֧����֧��

            if (AlipayAppService == null)
            {
                throw new UserFriendlyException("֧��δ���ţ�����ϵ����Ա��");
            }
            var appPayInput = new Alipay.Dto.AppPayInput
            {
                Body = input.Body,
                Subject = input.Subject,
                TradeNo = input.OutTradeNo,
                PassbackParams = input.CustomData,
                TotalAmount = input.TotalAmount
            };
            try
            {
                var appPayOutput = await AlipayAppService.AppPay(appPayInput);
                return appPayOutput.Response.Body;
            }
            catch (Exception ex)
            {
                throw new UserFriendlyException(ex.Message);
            }

## ֧��������֧��


### �ο�����
            if (GlobalAlipayAppService == null)
            {
                throw new UserFriendlyException("֧��δ���ţ�����ϵ����Ա��");
            }
            var payInput = new Alipay.Global.Dto.PayInput
            {
                Body = input.Body,
                Subject = input.Subject,
                TradeNo = input.OutTradeNo,
                //PassbackParams = input.CustomData,
                TotalFee = input.TotalAmount,
            };
            try
            {
                return await GlobalAlipayAppService.Pay(payInput);
            }
            catch (Exception ex)
            {
                throw new UserFriendlyException(ex.Message);
            }

### ����

<img src="res/14.png">

## ֧���ص�

### Ŀ��
ͳһ�ص������߼��ͻص������ַ

### ����ο�

            void PayAction(string key, string outTradeNo, string transactionId, int totalFee, JObject data)
            {
                using (var paymentCallbackManagerObj = iocManager.ResolveAsDisposable<PaymentCallbackManager>())
                {
                    var paymentCallbackManager = paymentCallbackManagerObj?.Object;
                    if (paymentCallbackManager == null)
                    {
                        throw new ApplicationException("֧���ص��������쳣���޷�ִ�лص���");
                    }
                    AsyncHelper.RunSync(async () => await paymentCallbackManager.ExecuteCallback(key, outTradeNo, transactionId, totalFee, data));
                }
            }

<img src="res/20.png">

�ص�������ο��˴���:https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Startup/PayStartup.cs

