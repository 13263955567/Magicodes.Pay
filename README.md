# Magicodes.Pay

## ��Ҫ����
Magicodes.Pay���������Ƽ��Ŷ��ṩ��ͳһ֧���⣬��ؿ��ʹ��.NET��׼���д��֧��.NET Framework�Լ�.NET Core��Ŀǰ֧������֧����ʽ�͹��ܣ�
* ֧����APP֧��
* ֧����Wap֧��
* ֧��������֧��
  * ֧�ַ���
* ΢��С����֧��
* ΢��APP֧��
* ͳһ֧���ص�����
* ֧����־����ע�루������֧���⣩
* ֧��֧�����ú���ע�룬�Ա���֧���Զ������û�ȡ�߼�����Ӧ���ڲ�ͬ�ĳ���������������ļ����û����û�ȡ���ã����߶��⻧֧�֣�

Ŀǰ�˿������ںܶ���Ŀ���Ѿ���������֤��������Ŀ�Ϲ�����๦�����ǲ�û����ӡ�Ǩ�ƻ����ع��������ں����Ĺ����У����ǻ����������Щ������ͬʱ����Magicodes.Admin��Դ���У�����Ҳ��д����ص�Demo��ʵ�֡�

Magicodes.Admin��Դ���ַ��https://gitee.com/xl_wenqiang/Magicodes.Admin.Core

����֧��ʵ����飬������Magicodes.Admin��Դ�����Ѿ��ṩ��ͳһ֧����Demo���������ǽ����ṩ��������ͷ���Զ��������֧���Ĺ��ܡ�����ͼ��ʾ��
![�ο�](res/40.png)

�ڸ���ҵ��֧�������У����ǿ��Էǳ�����ĵ��ô�ͳһ֧��������ͼ��ʾ��
![�ο�](res/41.png)
![�ο�](res/42.png)

## VNext

����Ŀǰ���¸��汾�Ĺ滮��

* ֧����PC֧��
* ΢��H5֧��
* �ṩĬ�ϵĻص������߼���֧�ֻص���������ע��

���幦�����ǻ������Ŀ�������������������кõĽ��������������Թ�ע���ǵĹ��ںš�magiccodes�����ύ����������������

## �������

��ؿ��������ԱȽϼ򵥣�һ���ʹ�����Builder���������Զ�����־�߼������û�ȡ�߼��ȣ�������Բ���BuilderĿ¼�µĴ��롣

### ���òο�

���������ο��˴���:https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Startup/PayStartup.cs

���ִ���������ʾ��

![�ο�](res/1.png)
![�ο�](res/2.png)

֧����ش�����Բο�:
https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Services/PayAppService.cs

### ���ý���ο�

����ͼ��ʾ:
![�ο�](res/10.png)
![�ο�](res/11.png)
![�ο�](res/12.png)

## Demo

### ΢��֧��Demo

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

### ֧����֧��Demo

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

### ֧��������֧��Demo

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


### ����֧�������˲ο�����

![�ο�](res/14.png)

## ֧���ص�

### Ŀ��

ͳһ�ص������߼��ͻص������ַ

### ����ο�


![�ο�](res/20.png)

��ͼ��PayAction�ο���

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

�����ص�������ο��˴���:https://gitee.com/xl_wenqiang/Magicodes.Admin.Core/blob/develop/src/unity/Magicodes.Pay/Startup/PayStartup.cs

�ص��߼��ο�:
![�ص��߼�](res/30.png)

## �ٷ����ĺ�

��ע��magiccodes�����ĺ���ѻ�ȡ��

* �������¡��̡̳��ĵ�
* ��Ƶ�̳�
* �����������Ȩ
* ģ��
* �������
* ����ĵú�����

![�ٷ����ĺ�](res/wechat.jpg)

## ���QQȺ

��̽���Ⱥ<85318032>

��Ʒ����Ⱥ<897857351>

## �ٷ�����

<http://www.cnblogs.com/codelove/>

## ������Դ���ַ

<https://gitee.com/xl_wenqiang/Magicodes.Admin.Core>
<https://github.com/xin-lai>

