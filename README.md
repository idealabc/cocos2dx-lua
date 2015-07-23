# cocos2dx+lua 学习

1.用cJSON  解析JSON

2.创建一个文字标签
    
   	CCLabelTTF* label = CCLabelTTF::create(title().c_str(), "Arial", 32);
   	addChild(label, 1);
   	label->setPosition( ccp(VisibleRect::center().x, VisibleRect::top().y-50) );        


3.创建一个spring

    
    m_pBall = CCSprite::create("Images/ball.png");
    m_pBall->setPosition(ccp(VisibleRect::center().x, VisibleRect::center().y));
    addChild(m_pBall);

    
4.获取舞台

   
    CCDirector* pDir = CCDirector::sharedDirector();
    
        
5.获取内容的大小和位置

    	CCSize ballSize  = m_pBall->getContentSize();
    	CCPoint ptNow  = m_pBall->getPosition();
    
6.切换场景

    CCDirector::sharedDirector()->replaceScene(this);    
    
7.陀螺仪调用

 	 setAccelerometerEnabled(true);
 	 void AccelerometerTest::didAccelerate(CCAcceleration* pAccelerationValue)

  
8.创建一组菜单
 
 
    CCMenuItemImage *item1 = CCMenuItemImage::create(s_pPathB1, s_pPathB2, this, menu_selector(ActionManagerTest::backCallback) );
    CCMenuItemImage *item2 = CCMenuItemImage::create(s_pPathR1, s_pPathR2, this, menu_selector(ActionManagerTest::restartCallback) );
    CCMenuItemImage *item3 = CCMenuItemImage::create(s_pPathF1, s_pPathF2, this, menu_selector(ActionManagerTest::nextCallback));

    CCMenu *menu = CCMenu::create(item1, item2, item3, NULL);

    menu->setPosition(CCPointZero);
    item1->setPosition(ccp(VisibleRect::center().x - item2->getContentSize().width*2, VisibleRect::bottom().y + item2->getContentSize().height/2));
    item2->setPosition(ccp(VisibleRect::center().x, VisibleRect::bottom().y + item2->getContentSize().height/2));
    item3->setPosition(ccp(VisibleRect::center().x + item2->getContentSize().width*2, VisibleRect::bottom().y + item2->getContentSize().height/2));
    
    addChild(menu, 1);   
    ```
9.定时任务计度

	schedule( schedule_selector(PauseTest::unpause), 3); 
	
	void PauseTest::unpause(float dt)
	{
    	unschedule( schedule_selector(PauseTest::unpause) );
    	CCNode* node = getChildByTag( kTagGrossini );
    	CCDirector* pDirector = CCDirector::sharedDirector();
    	pDirector->getActionManager()->resumeTarget(node);
	}
    

10.加动画效果，加回调函数

	 CCMoveBy* pMove = CCMoveBy::create(2, ccp(200, 0));
    CCCallFunc* pCallback = CCCallFunc::create(this, callfunc_selector(RemoveTest::stopAction));
    CCActionInterval* pSequence = CCSequence::create(pMove, pCallback, NULL);
    pSequence->setTag(kTagSequence);

    CCSprite* pChild = CCSprite::create(s_pPathGrossini);
    pChild->setPosition( VisibleRect::center() );

    addChild(pChild, 1, kTagGrossini);
    pChild->runAction(pSequence);
    
    
11.循环动画

	CCActionInterval* move = CCMoveBy::create(3, ccp(VisibleRect::right().x-130, 0));
    CCActionInterval* move_back = move->reverse();
    
    CCActionInterval* move_ease = CCEaseBounceInOut::create((CCActionInterval*)(move->copy()->autorelease()) );
    CCActionInterval* move_ease_back = move_ease->reverse();
    
    CCDelayTime *delay = CCDelayTime::create(0.25f);
    
    CCSequence* seq1 = CCSequence::create(move, delay, move_back, CCCA(delay), NULL);
    CCSequence* seq2 = CCSequence::create(move_ease, CCCA(delay), move_ease_back, CCCA(delay), NULL);
    
    this->positionForTwo();
    
    m_grossini->runAction( CCRepeatForever::create(seq1));
    m_tamara->runAction( CCRepeatForever::create(seq2));
    
12.颜色层

	CCLayerColor *box = CCLayerColor::create(ccc4(255, 255, 0, 255));
    box->setAnchorPoint(ccp(0, 0));


13.按tag操作对像
	
	 addChild(sprite, 0, kTagSprite); //add时添加tag
	 
	CCNode* s = getChildByTag(kTagSprite);

14.手势

	virtual void ccTouchesBegan(CCSet *pTouches, CCEvent *pEvent) {CC_UNUSED_PARAM(pTouches); CC_UNUSED_PARAM(pEvent);}
     virtual void ccTouchesMoved(CCSet *pTouches, CCEvent *pEvent) {CC_UNUSED_PARAM(pTouches); CC_UNUSED_PARAM(pEvent);}
     virtual void ccTouchesEnded(CCSet *pTouches, CCEvent *pEvent) {CC_UNUSED_PARAM(pTouches); CC_UNUSED_PARAM(pEvent);}
     virtual void ccTouchesCancelled(CCSet *pTouches, CCEvent *pEvent) {CC_UNUSED_PARAM(pTouches); CC_UNUSED_PARAM(pEvent);}


	void MainLayer::ccTouchesEnded(CCSet *pTouches, CCEvent *pEvent)
	{
    CCSetIterator it = pTouches->begin();
    CCTouch* touch = (CCTouch*)(*it);
    
    CCPoint location = touch->getLocation();

    CCNode* s = getChildByTag(kTagSprite);
    s->stopAllActions();
    s->runAction( CCMoveTo::create(1, ccp(location.x, location.y) ) );
    float o = location.x - s->getPosition().x;
    float a = location.y - s->getPosition().y;
    float at = (float) CC_RADIANS_TO_DEGREES( atanf( o/a) );
    
    if( a < 0 ) 
    {
        if(  o < 0 )
            at = 180 + fabs(at);
        else
            at = 180 - fabs(at);    
    }
    
    s->runAction( CCRotateTo::create(1, at) );
	}


15.获取舞台大小

	CCSize s = CCDirector::sharedDirector()->getWinSize();
	

16.加载配置

	CCConfiguration::sharedConfiguration()->loadConfigFile("configs/config-test-ok.plist");
	CCConfiguration::sharedConfiguration()->dumpInfo();
	
	CCLOG("cocos2d version: %s", CCConfiguration::sharedConfiguration()->getCString("cocos2d.version") );
	CCLOG("OpenGL version: %s", CCConfiguration::sharedConfiguration()->getCString("gl.version") );
	
	const char *c_value = CCConfiguration::sharedConfiguration()->getCString("invalid.key", "no key");
	
	bool b_value = CCConfiguration::sharedConfiguration()->getBool("invalid.key", true);
	
	double d_value = CCConfiguration::sharedConfiguration()->getNumber("invalid.key", 42.42);
	
17.创建配置

	CCConfiguration *conf = CCConfiguration::sharedConfiguration();

	conf->setObject("this.is.an.int.value", CCInteger::create(10) );
	conf->setObject("this.is.a.bool.value", CCBool::create(true) );
	conf->setObject("this.is.a.string.value", CCString::create("hello world") );

	conf->dumpInfo();
	
18.CURL使用

	CURL *curl;
    CURLcode res;
    char buffer[10];

    curl = curl_easy_init();
    if (curl) 
    {
        curl_easy_setopt(curl, CURLOPT_URL, "baidu.com");
        res = curl_easy_perform(curl);
        /* always cleanup */
        curl_easy_cleanup(curl);
        if (res == 0)
        {
            m_pLabel->setString("0 response");
        }
        else
        {
            sprintf(buffer,"code: %i",res);
            m_pLabel->setString(buffer);
        }
    } 
    else 
    {
        m_pLabel->setString("no curl");
    } 

19.获取当前语言

	ccLanguageType currentLanguageType = CCApplication::sharedApplication()->getCurrentLanguage();
    switch (currentLanguageType)
    {
    case kLanguageEnglish:
        labelLanguage->setString("current language is English");
        
20.CCDataVisitor

	CCPrettyPrinter vistor;
    
    // print dictionary
    CCDictionary* pDict = CCDictionary::createWithContentsOfFile("animations/animations.plist");
    pDict->acceptVisitor(vistor);
    CCLog("%s", vistor.getResult().c_str());
    CCLog("-------------------------------");
    
    CCSet myset;
    for (int i = 0; i < 30; ++i) {
        myset.addObject(CCString::createWithFormat("str: %d", i));
    }
    vistor.clear();
    myset.acceptVisitor(vistor);
    CCLog("%s", vistor.getResult().c_str());
    CCLog("-------------------------------");
    
    vistor.clear();
    addSprite();
    pDict = CCTextureCache::sharedTextureCache()->snapshotTextures();
    pDict->acceptVisitor(vistor);
    CCLog("%s", vistor.getResult().c_str());
    
    
21.画图形

	
	CHECK_GL_ERROR_DEBUG();
    
	// draw a simple line
	// The default state is:
	// Line Width: 1
	// color: 255,255,255,255 (white, non-transparent)
	// Anti-Aliased
    //	glEnable(GL_LINE_SMOOTH);
    ccDrawLine( VisibleRect::leftBottom(), VisibleRect::rightTop() );
    
	CHECK_GL_ERROR_DEBUG();
    
	// line: color, width, aliased
	// glLineWidth > 1 and GL_LINE_SMOOTH are not compatible
	// GL_SMOOTH_LINE_WIDTH_RANGE = (1,1) on iPhone
    //	glDisable(GL_LINE_SMOOTH);
	glLineWidth( 5.0f );
	ccDrawColor4B(255,0,0,255);
    ccDrawLine( VisibleRect::leftTop(), VisibleRect::rightBottom() );
    
	CHECK_GL_ERROR_DEBUG();
    
	// TIP:
	// If you are going to use always the same color or width, you don't
	// need to call it before every draw
	//
	// Remember: OpenGL is a state-machine.
    
	// draw big point in the center
	ccPointSize(64);
	ccDrawColor4B(0,0,255,128);
    ccDrawPoint( VisibleRect::center() );
    
	CHECK_GL_ERROR_DEBUG();
    
	// draw 4 small points
	CCPoint points[] = { ccp(60,60), ccp(70,70), ccp(60,70), ccp(70,60) };
	ccPointSize(4);
	ccDrawColor4B(0,255,255,255);
	ccDrawPoints( points, 4);
    
	CHECK_GL_ERROR_DEBUG();
    
	// draw a green circle with 10 segments
	glLineWidth(16);
	ccDrawColor4B(0, 255, 0, 255);
    ccDrawCircle( VisibleRect::center(), 100, 0, 10, false);
    
	CHECK_GL_ERROR_DEBUG();
    
	// draw a green circle with 50 segments with line to center
	glLineWidth(2);
	ccDrawColor4B(0, 255, 255, 255);
    ccDrawCircle( VisibleRect::center(), 50, CC_DEGREES_TO_RADIANS(90), 50, true);
    
	CHECK_GL_ERROR_DEBUG();
    
	// open yellow poly
	ccDrawColor4B(255, 255, 0, 255);
	glLineWidth(10);
	CCPoint vertices[] = { ccp(0,0), ccp(50,50), ccp(100,50), ccp(100,100), ccp(50,100) };
	ccDrawPoly( vertices, 5, false);
    
	CHECK_GL_ERROR_DEBUG();
	
	// filled poly
	glLineWidth(1);
	CCPoint filledVertices[] = { ccp(0,120), ccp(50,120), ccp(50,170), ccp(25,200), ccp(0,170) };
	ccDrawSolidPoly(filledVertices, 5, ccc4f(0.5f, 0.5f, 1, 1 ) );
    
    
	// closed purble poly
	ccDrawColor4B(255, 0, 255, 255);
	glLineWidth(2);
	CCPoint vertices2[] = { ccp(30,130), ccp(30,230), ccp(50,200) };
	ccDrawPoly( vertices2, 3, true);
    
	CHECK_GL_ERROR_DEBUG();
    
	// draw quad bezier path
    ccDrawQuadBezier(VisibleRect::leftTop(), VisibleRect::center(), VisibleRect::rightTop(), 50);
    
	CHECK_GL_ERROR_DEBUG();
    
	// draw cubic bezier path
    ccDrawCubicBezier(VisibleRect::center(), ccp(VisibleRect::center().x+30,VisibleRect::center().y+50), ccp(VisibleRect::center().x+60,VisibleRect::center().y-50),VisibleRect::right(),100);
    
	CHECK_GL_ERROR_DEBUG();
    
    //draw a solid polygon
	CCPoint vertices3[] = {ccp(60,160), ccp(70,190), ccp(100,190), ccp(90,160)};
    ccDrawSolidPoly( vertices3, 4, ccc4f(1,1,0,1) );
    
	// restore original values
	glLineWidth(1);
	ccDrawColor4B(255,255,255,255);
	ccPointSize(1);
    
	CHECK_GL_ERROR_DEBUG();
	
22.效果

	 CCFlipX3D* flipx  = CCFlipX3D::create(t);
        CCActionInterval* flipx_back = flipx->reverse();
        CCDelayTime* delay = CCDelayTime::create(2);
        
        return CCSequence::create(flipx, delay, flipx_back, NULL);
        
        
23.HTTPClient

	CCHttpRequest* request = new CCHttpRequest();
        request->setUrl("http://www.baidu.com");
        request->setRequestType(CCHttpRequest::kHttpPost);
        request->setResponseCallback(this, httpresponse_selector(HttpClientTest::onHttpRequestCompleted));
        
        // write the post data
        const char* postData = "visitor=cocos2d&TestSuite=Extensions Test/NetworkTest";
        request->setRequestData(postData, strlen(postData)); 
        
        request->setTag("POST test1");
        CCHttpClient::getInstance()->send(request);
        request->release();
        
        
        
        if (0 != strlen(response->getHttpRequest()->getTag())) 
    {
        CCLog("%s completed", response->getHttpRequest()->getTag());
    }
    
    int statusCode = response->getResponseCode();
    
    std::vector<char> *buffer = response->getResponseData();
    printf("Http Test, dump data: ");
    for (unsigned int i = 0; i < buffer->size(); i++)
    {
        printf("%c", (*buffer)[i]);
    }

	
	
24.WebSocket

	
	 _wsiSendText = new WebSocket();
    
    if (!_wsiSendText->init(*this, "ws://10.2.73.202:8082/s"))
    {
        CC_SAFE_DELETE(_wsiSendText);
    }
    
	virtual void onOpen(WebSocket* ws) = 0;
    virtual void onMessage(WebSocket* ws, const Data& data) = 0;
    virtual void onClose(WebSocket* ws) = 0;
    virtual void onError(WebSocket* ws, const ErrorCode& error) = 0;

25.消息中心
	
	//注册
	CCNotificationCenter::sharedNotificationCenter()->addObserver(this, callfuncO_selector(Light::switchStateChanged), MSG_SWITCH_STATE, NULL);
	
	//移除
	CCNotificationCenter::sharedNotificationCenter()->removeObserver(this, MSG_SWITCH_STATE);
	//发送
	CCNotificationCenter::sharedNotificationCenter()->postNotification(MSG_SWITCH_STATE, (CCObject*)(intptr_t)item->getSelectedIndex());

26.TABLEVIEW

	virtual void scrollViewDidScroll(cocos2d::extension::CCScrollView* view) {};
    virtual void scrollViewDidZoom(cocos2d::extension::CCScrollView* view) {}
    virtual void tableCellTouched(cocos2d::extension::CCTableView* table, cocos2d::extension::CCTableViewCell* cell);
    virtual cocos2d::CCSize tableCellSizeForIndex(cocos2d::extension::CCTableView *table, unsigned int idx);
    virtual cocos2d::extension::CCTableViewCell* tableCellAtIndex(cocos2d::extension::CCTableView *table, unsigned int idx);
    virtual unsigned int numberOfCellsInTableView(cocos2d::extension::CCTableView *table);
    
    
27.文件工具

	CCFileUtils *sharedFileUtils = CCFileUtils::sharedFileUtils();
    
    string ret;
    
    sharedFileUtils->purgeCachedEntries();
    m_defaultSearchPathArray = sharedFileUtils->getSearchPaths();
    vector<string> searchPaths = m_defaultSearchPathArray;
    string writablePath = sharedFileUtils->getWritablePath();
    string fileName = writablePath+"external.txt";
    char szBuf[100] = "Hello Cocos2d-x!";
    FILE* fp = fopen(fileName.c_str(), "wb");
    if (fp)
    {
        fwrite(szBuf, 1, strlen(szBuf), fp);
        fclose(fp);
        CCLog("Writing file to writable path succeed.");
    }

28.字体测试

	CCLabelTTF *center = CCLabelTTF::create("alignment center", pFont, fontSize,
                                            blockSize, kCCTextAlignmentCenter, verticalAlignment[vAlignIdx]);
                                            
29.太阳粒子

	CCSize s = CCDirector::sharedDirector()->getWinSize();
    // sun
    CCParticleSystem* sun = CCParticleSun::create();
    sun->setTexture(CCTextureCache::sharedTextureCache()->addImage("Images/fire.png"));
    sun->setPosition( ccp(VisibleRect::rightTop().x-32,VisibleRect::rightTop().y-32) );

    sun->setTotalParticles(130);
    sun->setLife(0.6f);
    this->addChild(sun);
    
30.fnt字体

	m_label0 = CCLabelBMFont::create("0", "fonts/bitmapFontTest4.fnt");
	
31.输入框

	CCSize s = CCDirector::sharedDirector()->getWinSize();
    CCLabelTTF* label = CCLabelTTF::create("Keypad Test", "Arial", 28);
    addChild(label, 0);
    label->setPosition( ccp(s.width/2, s.height-50) );

    setKeypadEnabled(true);

32.Label测试

	CCLabelAtlas* label1 = CCLabelAtlas::create("123 Test", "fonts/tuffy_bold_italic-charmap.plist");
    addChild(label1, 0, kTagSprite1);
    label1->setPosition( ccp(10,100) );
    label1->setOpacity( 200 );
    
    
    CCLabelAtlas* label1 = CCLabelAtlas::create("123 Test", "fonts/tuffy_bold_italic-charmap.png", 48, 64, ' ');
    addChild(label1, 0, kTagSprite1);
    label1->setPosition( ccp(10,100) );
    label1->setOpacity( 200 );



	CCLabelBMFont* label1 = CCLabelBMFont::create("Test",  "fonts/bitmapFontTest2.fnt");                                           
	
	CCLabelBMFont* label = NULL;
    label = CCLabelBMFont::create("Blue", "fonts/bitmapFontTest5.fnt");
    
    
33.加载配置

	 CCDictionary *strings = CCDictionary::createWithContentsOfFile("fonts/strings.xml");

    const char *chinese  = ((CCString*)strings->objectForKey("chinese1"))->m_sString.c_str();
    const char *japanese = ((CCString*)strings->objectForKey("japanese"))->m_sString.c_str();
    const char *russian  = ((CCString*)strings->objectForKey("russian"))->m_sString.c_str();
    const char *spanish  = ((CCString*)strings->objectForKey("spanish"))->m_sString.c_str();
    
34.层

	CCLayerRGBA* layer1 = CCLayerRGBA::create();
	
35.地图
	
	CCTileMapAtlas* tilemap = CCTileMapAtlas::create(s_TilesPng, s_LevelMapTga, 16, 16);
    tilemap->releaseMap();
    
    // change the transform anchor to 0,0 (optional)
    tilemap->setAnchorPoint( ccp(0, 0) );
    
    // Anti Aliased images
    tilemap->getTexture()->setAntiAliasTexParameters();
    
36.粒子

	 m_emitter = CCParticleFireworks::create();
    m_emitter->retain();
    m_background->addChild(m_emitter, 10);
    
    m_emitter->setTexture( CCTextureCache::sharedTextureCache()->addImage(s_stars1) );
    
37.纹理渲染

	m_pTarget = CCRenderTexture::create(s.width, s.height, kCCTexture2DPixelFormat_RGBA8888);
    m_pTarget->retain();
    m_pTarget->setPosition(ccp(s.width / 2, s.height / 2));


	CCImage *pImage = m_pTarget->newCCImage();

    CCTexture2D *tex = CCTextureCache::sharedTextureCache()->addUIImage(pImage, png);

    CC_SAFE_DELETE(pImage);

    CCSprite *sprite = CCSprite::createWithTexture(tex);

	

38.CCSpriteFrameCache Spring 缓存

	CCSpriteFrameCache::sharedSpriteFrameCache()->addSpriteFramesWithFile("Images/bugs/circle.plist");
    mgr = CCSpriteBatchNode::create("Images/bugs/circle.png", 9);
    this->addChild(mgr);
    sp1 = CCSprite::createWithSpriteFrameName("circle.png");
    
39.加载图片资源

	CCTextureCache::sharedTextureCache()->addImageAsync("Images/HelloWorld.png", this, callfuncO_selector(TextureCacheTest::loadingCallBack));
	
40.CCTileMapAtlas

	CCTileMapAtlas* map = CCTileMapAtlas::create(s_TilesPng,  s_LevelMapTga, 16, 16);
    // Convert it to "alias" (GL_LINEAR filtering)
    map->getTexture()->setAntiAliasTexParameters();
    
41.CCArray
	
	CCArray *paddlesM = CCArray::createWithCapacity(4);
	
42.CCUserDefault

	   CCUserDefault::sharedUserDefault()->setStringForKey("string", "value1");
    CCUserDefault::sharedUserDefault()->setIntegerForKey("integer", 10);
    CCUserDefault::sharedUserDefault()->setFloatForKey("float", 2.3f);
    CCUserDefault::sharedUserDefault()->setDoubleForKey("double", 2.4);
    CCUserDefault::sharedUserDefault()->setBoolForKey("bool", true);


43.





























