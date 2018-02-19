# Targeting

Targeting values get passed in each ad call, and help the ad server choose the
most appropriate ad for the user/page/context.

Targeting can be added for the whole `page` or for individual `slots`.

Typical targeting can look like:

```
const targets = {
  env_device_type: device,
  rdt_device_template: `${device}_${template_type}`,
  ctx_template: template_type,
  ctx_page_channel: channel,
  ctx_page_sub_channel: sub_channel,
  ctx_page_slug: slug,
  env_server: server,
  cnt_tags: tags
};
```

The targets will get sent along as query parameters in the ad request.

## Events

Page level targeting: `ads.page.targeting.added`

Slot level targeting: `ads.slot.targeting.added`



### PAGE TARGETING VALUES
```
env_device_type
rdt_device_template
ctx_template
ctx_page_channel
ctx_page_sub_channel
ctx_page_slug
env_server
cnt_tags
env_device_type
ctx_cns_version
ctx_ses_soc
ctx_ref_soc
ctx_ref_url
usr_bkt_eva
usr_bkt_ses
usr_bkt_pv
ctx_ses_soc
usr_pvc_bs
usr_pvc_24hr
usr_pvc_30d
usr_svc_30d
mbid

// legacy targeting (v4)
sqt
sqt24hrs
mbid
```

### SLOT TARGETING VALUES
```
ctx_slot_instance
ctx_slot_type
ctx_slot_name
ctx_slot_rn
ctx_slot_manual_rn
ctx_advertisers
ctx_line_items
ctx_creatives
```


