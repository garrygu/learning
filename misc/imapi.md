# Overview
`ItemMaintain.RestfulAPI.Business.PricingBiz.Item.Seller`   
  -> `class SellerAllPriceBiz`  
    -> `class SellerPriceProcessor`

# Class SellerPriceProcessor
## Properties
```
SellerAllPriceRequest bizRequest;

private string itemPriceColumns;

private List<ItemBaseBDEntity> itemBD;
private List<ItemPriceBDEntity> priceBD;
private List<ItemPriceBDEntity> parentPriceBD;
private List<ItemPriceBDEntity> syncRestrictedMarkPriceBD;
private List<ItemBaseBDEntity> parentItemBD;
private List<SubCategoryHashBDEntity> subcategoryBD;
private List<ManufacturerHashBDEntity> manufacturerBD;

private List<BusinessUnitEntity> countryList;
private List<PromotionEntity> promotionBufferLockList;//maybe status in pending or active
private List<PromotionEntity> activePriceChangeList;//price adjust and promotion
private List<PromotionEntity> promotionList;//only promotion
private List<GetSellerReturnPolicyResponse> returnPolicyList;

private Dictionary<string, string> shipByNeweggException = new Dictionary<string, string>();
private List<HazmatDataPointItem> _HazmatDataPoint = new List<HazmatDataPointItem>();

private DateTime serverTime;

private bool isGetBigDataError;
private bool isSaveBigDataError;

private int numberOfThreadNotYetCompleted;
private AutoResetEvent autoEvent;

private List<AllPriceUpdateEntity> updateList = new List<AllPriceUpdateEntity>();
private List<AllPriceUpdateEntity> syncRestrictedList = new List<AllPriceUpdateEntity>();
private List<ItemInformationChangeLog> logList = new List<ItemInformationChangeLog>();
private ProcessStatusSeller response;
private SellerAllPriceProcessResponse processResponse;
private List<SellerAllPriceEntity> removeList;

private List<CalculatorParameter> defaultShippingParaList = new List<CalculatorParameter>();
private List<CalculatorParameter> eggSaverShippingParaList = new List<CalculatorParameter>();
private List<CalculatorParameter> shipBySellerShippingParaList = new List<CalculatorParameter>();

private Dictionary<string, decimal> defaultShippingList;
private Dictionary<string, decimal> eggSaverShippingList;
private Dictionary<string, ItemDefaultShippingMethod> shippingByMKPLList;

private List<ItemPriceBDEntity> priceBDSaveList = new List<ItemPriceBDEntity>();
private List<ItemBaseBDEntity> itemBDSaveList = new List<ItemBaseBDEntity>();

private bool messageFlag;
private string messageName;

private string memo = string.Empty;
private List<LogEntity> timeLog = new List<LogEntity>();
private int requestCNT = 0;

private List<SellerEnforceMAPSQLEntity> enforceMAPList = new List<SellerEnforceMAPSQLEntity>();
```

## Methods
### public ProcessStatusSeller Handle()
- SpentTimeLog(true, "Total");
- PrepareCheckData();
- BizCheck();
- PrepareSaveData();
- Save();
- SpentTimeLog(false, "Total");
- PrintLog();

### public SellerAllPriceProcessResponse ProcessHandle()
- SpentTimeLog(true, "Total");
- PrepareCheckData();
- BizCheck();
- PrepareSaveData();
- SendMQ2Submit();
- SpentTimeLog(false, "Total");
- PrintLog();


### public ProcessStatusSeller SubmitHandle()
- SpentTimeLog(true, "Total");
- response = new ProcessStatusSeller() { ErrorStatusList = new List<ErrorStatus>() };
- Save();
- SpentTimeLog(false, "Total");
- PrintLog();


### `private List<SellerAllPriceEntity> CombineRequest(List<SellerAllPriceEntity> srcList)` (p221)
- Combine requests by item# +countryCode + companyCode   
- Fields combined in `SellerAllPriceEntity`
  - UnitPrice
  - InstantRebate
  - MAPPrice
  - IsCheckoutMAP
  - MinimumQuantity
  - ShippingType
  - ShippingCharge
  - DefaultShipVia
  - IsSBN
  - IsShippingRestriction
  - IsActive
  - IsWebsiteblockmark
  - RequestUnitPrice
  - RequestDiscountInstant
  - RequestMAPPrice
  - RequestCurrencyCode
  - ExchangeRate
  - PriceAutoAdjustMark
- Questions:
  - Need ordering by time?

### private void InitTimeLogList()
### private void InitProcessTimeLogList()
### private void InitSubmitTimeLogList()

### private void PrepareCheckData() (p372)
- itemPriceColumns
- 1st Get  
  - GetItemBD
  - GetPriceBD
  - GetCountry
  - GetServerTime
  - GetSBNException
  - GetPromotion
- MultiTask(firstGetList, "1st Get");
- SetBizInfo();
- Check whether price has changes
  - IsSBN
  - IsNew
  - IsShippingRestriction
  - IsActive
  - UnitPrice
  - MAPPrice
  - IsCheckoutMAP
- 2nd Get
  * If parent check needed
    * GetParentPriceBD
    * GetParentItemBD
    * GetEnforceConfig
  * GetSubcategoryBD
  * GetManufacturerBD
  * GetReturnPolicy
  * GetHAZMAT
-  MultiTask(secondGetList, "2nd Get");
- check: IsShippingRestriction has value
- 3rd Get
  * GetSyncRestrictedMarkPriceBD
- MultiTask(thirdGetList, "3rd Get");

### private void BizCheck() (p428)
- isGetBigDataError
- request.bizInfo.IsDataIncorrect
- country==null
- string.IsNullOrEmpty(request.RequestCurrencyCode)
  * RequestCurrencyCode
- itemBD==null
- SellerItemNumber==null || SellerItemNumber.DeleteFlag == true
- request.bizInfo.IsNew == false && price == null  (priceBD:SellerItemNumber+CountryCode+CompanyCode )
- request.bizInfo.IsNew == false && NoChangeCheck(request, item, price)
- request.IsActive == true
  * string.IsNullOrEmpty(item.ImageName) == true    //03302028003
  * unitPrice == 99999      //03302028004
  * subcategory == null || subcategory.Active == false  //03302028015
  * manufactory == null || manufactory.Active == false  //03302028016
- request.IsActive == false
  * priceChangeBuffer != null   //on promotion schedule, 03302028005
- request.IsWebsiteblockmark.HasValue
  * (request.IsWebsiteblockmark == true) && ((!request.IsActive.HasValue && price.Active == true) || (request.IsActive.HasValue && request.IsActive == true))
- request.IsSBN == true
  * subcategory != null && subcategory.ShipByNewegg == false    //03302028006
  * shipByNeweggException != null
    * shippingException.Value == "0"    //03302028007
  * parentPriceBD != null && parentPriceBD.Count > 0
    * ((bool)parentRestrictedItemMark || (bool)parentAITMark) && (request.bizInfo.IsINTLCountry)    //03302028018
- request.ShippingType.HasValue
  * (request.ShippingType == 1 && request.bizInfo.IsSBN == true && request.bizInfo.IsINTLCountry    //03302028008
- request.IsShippingRestriction.HasValue && request.IsShippingRestriction != orgRestrictedItemMark
  * request.IsShippingRestriction == false    //03302028009
  * else
    * parentPriceBD != null && parentPriceBD.Count > 0    
      * parentPrice != null     //03302028010
- request.MinimumQuantity.HasValue
  * request.MinimumQuantity > orgLimitQuantity      //03302028011
  * else: price != null && price.PromotionQTY1 > 0 && request.MinimumQuantity > price.PromotionQTY1     //03302028012
- request.bizInfo.IsNew
  * returnPolicy == null    //03302028013
  * parentPriceBD != null && parentPriceBD.Count > 0
    * parentPrice == null       //03302028014
  * else    ///03302028014
