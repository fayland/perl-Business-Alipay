# NAME

Business::Alipay - Alipay payment

# SYNOPSIS

    use Business::Alipay;

    my $alipay = Business::Alipay->new({
	"ALIPAY_KEY" => "", # 安全检验码，以数字和字母组成的32位字符
      "ALIPAY_PARTNER" => "", # 合作身份者ID，以2088开头的16位纯数字
      "ALIPAY_SELLER_EMAIL" => "", # 签约支付宝账号或卖家支付宝帐户
    });

    my $redirect_url = $alipay->create_direct_pay_by_user({
      notify_url => $notify_url, ## notify url
      return_url => $return_url, ## 返回 url

      out_trade_no => $uuid, # unique id
      subject => 'subject',
      body => 'description',
      total_fee => '0.01', # 金额
    });
    # $redirect_url is a url, you should redirect to that
    print $q->redirect($redirect_url);

    #### for return or notify, in another sub or file
    my $params = $q->params(); # or $c->req->params->to_hash for Mojolicious

    my $out_trade_no = $params->{out_trade_no};
    # make sure it's not proceeded before.
    # return if already_proceeded($out_trade_no);

    my $trade_status = $params->{trade_status};
    if ($trade_status eq 'TRADE_FINISHED' or $trade_status eq 'TRADE_SUCCESS') {
	my $r = $alipay->notify_verify($params);
	# $r is one of 'true', 'false', 'sign_dismatch', 'unknown' (unknown is HTTP failure)
    }

# DESCRIPTION

Business::Alipay is a payment gateway for [https://www.alipay.com/](https://www.alipay.com/).

right now it's very incomplete and only supports __create\_direct\_pay\_by\_user__. patches are welcome.

# AUTHOR

Fayland Lam <fayland@gmail.com>

# COPYRIGHT

Copyright 2013- Fayland Lam

# LICENSE

This library is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.

# SEE ALSO
