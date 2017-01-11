# android开发中的坑

1, 启动服务
注意：为了确保应用的安全性，启动 Service 时，请始终使用显式 Intent，且不要为服务声明 Intent 过滤器。
使用隐式 Intent 启动服务存在安全隐患，因为您无法确定哪些服务将响应 Intent，且用户无法看到哪些服务已启动。
从 Android 5.0（API 级别 21）开始，如果使用隐式 Intent 调用 bindService()，系统会引发异常。

2， 设置Intent
 注意：若要同时设置 URI 和 MIME 类型，请勿调用 setData() 和 setType()，因为它们会互相抵消彼此的值。
 请始终使用 setDataAndType() 同时设置 URI 和 MIME 类型。

3， 隐式 Intent 示例
注意：用户可能没有任何应用处理您发送到 startActivity() 的隐式 Intent。如果出现这种情况，则调用将会失败，且应用会崩溃。
要验证 Activity 是否会接收 Intent，请对 Intent 对象调用 resolveActivity()。
如果结果为非空，则至少有一个应用能够处理该 Intent，且可以安全调用 startActivity()。 
如果结果为空，则不应使用该 Intent。如有可能，您应停用发出该 Intent 的功能。

    // Verify that the intent will resolve to an activity
    if (sendIntent.resolveActivity(getPackageManager()) != null) {
        startActivity(intent);
    }
 
4, 限制对组件的访问
使用 Intent 过滤器时，无法安全地防止其他应用启动组件。 
尽管 Intent 过滤器将组件限制为仅响应特定类型的隐式 Intent，但如果开发者确定您的组件名称，
则其他应用有可能通过使用显式 Intent 启动您的应用组件。如果必须确保只有您自己的应用才能启动您的某一组件，
请针对该组件将 exported 属性设置为 "false"。

