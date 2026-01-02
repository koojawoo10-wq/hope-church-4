import React, { useState, useEffect } from 'react';
import { ChevronRight, ChevronLeft, Heart, User, Check, Globe, Mic, Home, Star, Download, X, Share, Lock, PieChart, List, LogOut, CheckCircle, Settings, Edit3, Briefcase, Sparkles, HandHeart, MessageCircle, RefreshCw, FileText, Save, Quote, Smartphone, ArrowUpCircle, Menu, FileSpreadsheet, Mail } from 'lucide-react';

// 다국어 텍스트 데이터베이스
const TRANSLATIONS = {
  ko: {
    headerTitle: "가족사랑 영혼구원", 
    installBtn: "앱 설치", 
    introTitle: "새생명품기 대행진",
    introDesc1: "가정에 복음의 빛을!\n이웃에게 구원의 소망을!", 
    introDesc2: "그리스도의 사랑으로 뜨겁게 품고,\n생명을 살리는 구원의 대장정을\n성령충만으로 힘차게 시작합시다.",
    installGuideLink: "앱 설치하기", 
    notice: "💡 목회적 돌봄과 중보기도를 위해서만 사용됩니다.",
    startBtn: "대행진 참여하기",
    adminBtn: "목회자 관리 모드",
    prevBtn: "이전",
    nextBtn: "다음",
    reviewBtn: "입력 내용 확인",
    submitBtn: "최종 제출하기",
    submitCompleteTitle: "제출이 완료되었습니다!",
    submitCompleteDesc: "님을 위한\n중보기도가 시작됩니다.",
    anotherFamilyBtn: "다른 가족 등록하기",
    verse: "\"주 예수를 믿으라 그리하면\n너와 네 집이 구원을 받으리라\"\n(행 16:31)",
    
    // Step 1: 기본 정보
    step1Title: "1. 기본 신상 정보",
    labelSubmitter: "작성자(성도님) 성함",
    phSubmitter: "예: 이사랑권사", 
    descSubmitter: "* 성도님의 성함을 입력해주세요.",
    labelTarget: "전도 대상자 성명",
    phTarget: "예: 홍길동",
    labelGender: "성별",
    optionsGender: ["남성", "여성"],
    labelNation: "국적",
    optionsNation: ["대한민국", "일본", "중국", "기타"],
    labelLang: "주 사용 언어",
    optionsLang: ["한국어", "일본어", "한국어&일본어", "영어", "기타"],
    langMsg: "※ 일본어 전도 콘텐츠 및 신공동역(新共同訳) 성경이 자동 적용됩니다.",
    labelRel: "관계",
    optionsRel: ["배우자", "부모", "자녀", "형제/자매", "친척", "기타"],
    labelAge: "연령대",
    optionsAge: ["10대 미만", "10대", "20대", "30대", "40대", "50대", "60대", "70대", "80대 이상"],
    labelJob: "현재 직업",
    optionsJob: [
        "학생 (초/중/고/대)",
        "직장인/회사원/공무원",
        "자영업/사업",
        "전문직/프리랜서",
        "주부",
        "무직/취업준비",
        "은퇴/노후",
        "기타"
    ],
    labelLive: "거주 형태",
    optionsLive: ["독거 (혼자)", "동거 (함께)", "인근 거주", "타지역/해외"],
    labelReligion: "현재 종교",
    optionsReligion: ["기독교(가나안 성도)", "천주교", "불교", "신도교 (Shinto)", "무교", "기타"],
    selectPlaceholder: "선택해 주세요",

    // Step 2: 신앙 진단
    step2Title: "2. 신앙 상태 진단",
    labelFavor: "교회 호감도",
    favorLabels: ["매우 부정", "매우 우호"],
    labelExp: "과거 신앙 경험",
    optionsExp: ["경험 없음", "주일학교 출신", "세례 받음", "과거 출석(현재 중단)"],
    labelReject: "주된 거부/반응 이유",
    optionsReject: ["종교 강요에 대한 거부감", "타종교(신도/불교 등) 신념 확고", "제사 및 문화적 이유", "기독교인/교회에 대한 상처", "무관심 ('나중에 늙으면 가겠다')"],

    // Step 3: 접촉점
    step3Title: "3. 접촉점 파악",
    labelInterest: "주요 관심사 및 고민 (중복 가능)",
    optionsInterest: ["건강/질병", "자녀교육/진로", "사업/재정", "외로움/관계", "가정불화", "노후문제"],
    labelInvite: "가장 좋은 초청 계기",
    optionsInvite: ["부활절/성탄절 등 절기", "음악회/바자회 등 문화행사", "가정의 달 행사", "구역 식사 모임 초대", "목사님 심방/만남"],

    // Step 4: 기도
    step4Title: "4. 기도와 결단",
    labelPrayer: "구체적인 기도제목",
    phPrayer: "예: 남편의 사업 문제가 해결되어 마음의 여유를 갖고 하나님을 찾게 해주세요.",
    labelCommit: "나의 전도 다짐",
    optionsCommit: ["매일 1분 기도하기", "주 1회 사랑의 섬김 실천", "올해 안에 교회 뜰 밟게 하기"],
    labelNote: "참고 사항 (메모)",
    phNote: "기타 참고할 사항이나 목사님께 전할 말이 있다면 자유롭게 적어주세요.",

    // Step 5: 검토
    step5Title: "5. 입력 내용 확인",
    editBtn: "수정",
    reviewNotice: "입력하신 내용이 맞는지 확인해 주세요.\n수정이 필요하면 [수정] 버튼을 눌러주세요.",

    // Admin & Install
    adminMode: "목회자 관리 모드",
    exitAdmin: "나가기",
    totalVIP: "총 태신자",
    jpGroup: "일본어권",
    promising: "전도 유망",
    listTitle: "등록 명단",
    latestOrder: "최신순",
    installTitle: "홈 화면에 앱 추가하기", 
    iosGuide: "아이폰 (Safari)",
    iosStep1: "화면 하단의 [공유] 버튼을 눌러주세요.",
    iosStep2: "메뉴를 올려서 [홈 화면에 추가]를 선택하세요.",
    iosStep3: "우측 상단의 [추가] 버튼을 누르면 설치 완료!",
    androidGuide: "갤럭시/안드로이드 (Chrome)",
    androidStep1: "화면 우측 상단의 점 3개(⋮) 메뉴를 눌러주세요.",
    androidStep2: "[앱 설치] 또는 [홈 화면에 추가]를 선택하세요.",
    androidStep3: "[설치] 또는 [추가]를 누르면 완료됩니다.",
    adminPwTitle: "관리자 비밀번호 (0000)",
    cancel: "취소",
    confirm: "확인",
    
    // Admin Settings & Edit
    settingsTitle: "교회 정보 설정",
    churchNameKoLabel: "교회 이름 (한국어)",
    churchNameJaLabel: "교회 이름 (일본어)",
    saveBtn: "저장하기",
    saveSuccess: "저장되었습니다.",
    editSubmissionTitle: "등록 내용 수정",
    updateBtn: "수정 완료",
    
    // [추가] 엑셀/메일 관련
    exportExcelBtn: "엑셀(CSV) 저장",
    sendMailBtn: "메일 앱 열기",
    exportSuccessMsg: "파일이 다운로드되었습니다.\n이제 '메일 앱 열기'를 눌러 파일을 첨부하세요."
  },
  ja: {
    headerTitle: "家族愛 魂の救い", 
    installBtn: "アプリ追加",
    introTitle: "新しい命を抱く大行進", 
    introDesc1: "家庭に福音の光を！\n隣人に救いの望みを！", 
    introDesc2: "キリストの愛で熱く抱き、\n命を生かす救いの大行進を\n聖霊充満で力強く始めましょう。",
    installGuideLink: "アプリをインストール", 
    notice: "💡 牧会的ケアととりなしの祈りのためにのみ使用されます。", 
    startBtn: "大行進に参加する", 
    adminBtn: "牧会者管理モード",
    prevBtn: "戻る",
    nextBtn: "次へ",
    reviewBtn: "入力内容の確認",
    submitBtn: "最終提出",
    submitCompleteTitle: "提出が完了しました！",
    submitCompleteDesc: "様のための\nとりなしの祈りが始まります。",
    anotherFamilyBtn: "他の家族を登録する",
    verse: "\"主イエスを信じなさい。\nそうすれば、あなたも家族も救われます。\"\n(使徒 16:31)",
    
    // Step 1
    step1Title: "1. 基本情報",
    labelSubmitter: "作成者(信徒)のお名前",
    phSubmitter: "例：李愛 勧士",
    descSubmitter: "* ご自身のお名前を入力してください。",
    labelTarget: "伝道対象者のお名前",
    phTarget: "例：山田太郎",
    labelGender: "性別",
    optionsGender: ["男性", "女性"],
    labelNation: "国籍",
    optionsNation: ["大韓民国", "日本", "中国", "その他"],
    labelLang: "主な使用言語",
    optionsLang: ["韓国語", "日本語", "韓国語＆日本語", "英語", "その他"],
    langMsg: "※ 日本語の伝道コンテンツおよび新共同訳聖書が自動適用されます。",
    labelRel: "関係",
    optionsRel: ["配偶者", "父母", "子供", "兄弟/姉妹", "親戚", "その他"],
    labelAge: "年齢層",
    optionsAge: ["10代未満", "10代", "20代", "30代", "40代", "50代", "60代", "70代", "80代以上"],
    labelJob: "現在の職業",
    optionsJob: [
        "学生 (小/中/高/大)",
        "会社員/公務員",
        "自営業/事業",
        "専門職/フリーランス",
        "主婦/主夫",
        "無職/求職中",
        "定年/隠居",
        "その他"
    ],
    labelLive: "居住形態",
    optionsLive: ["独居(一人)", "同居(一緒)", "近隣在住", "他地域/海外"],
    labelReligion: "現在の宗教",
    optionsReligion: ["キリスト教(カナン聖徒)", "カトリック", "仏教", "神道 (Shinto)", "無宗教", "その他"],
    selectPlaceholder: "選択してください",

    // Step 2
    step2Title: "2. 信仰状態の診断",
    labelFavor: "教会への好感度",
    favorLabels: ["非常に否定", "非常に友好"],
    labelExp: "過去の信仰経験",
    optionsExp: ["経験なし", "教会学校出身", "洗礼を受けた", "過去出席(現在中断)"],
    labelReject: "主な拒否/反応の理由",
    optionsReject: ["宗教の強要への拒否感", "他宗教(神道/仏教等)の信念", "祭祀および文化的な理由", "キリスト教/教会への傷", "無関心(「年を取ったら行く」)"],

    // Step 3
    step3Title: "3. 接点（必要）の把握",
    labelInterest: "主な関心事および悩み (重複可)",
    optionsInterest: ["健康/病気", "子供の教育/進路", "事業/財政", "孤独/人間関係", "家庭不和", "老後の問題"],
    labelInvite: "最も良い招待のきっかけ",
    optionsInvite: ["イースター/クリスマス等", "音楽会/バザー等の文化行事", "家庭月間の行事", "区域(小グループ)の食事会", "牧師の訪問/面会"],

    // Step 4
    step4Title: "4. 祈りと決断",
    labelPrayer: "具体的な祈りの課題",
    phPrayer: "例：夫の事業の問題が解決され、心の余裕を持って神様を求めることができますように。",
    labelCommit: "私の伝道の誓い",
    optionsCommit: ["毎日1分祈る", "週1回愛の奉仕を実践", "今年中に教会の庭を踏ませる"],
    labelNote: "参考事項 (メモ)",
    phNote: "その他、参考事項や牧師に伝えたいことがあれば自由にご記入ください。",

    // Step 5
    step5Title: "5. 入力内容の確認",
    editBtn: "修正",
    reviewNotice: "入力内容が正しいか確認してください。\n修正が必要な場合は[修正]ボタンを押してください。",

    // Admin & Install
    adminMode: "牧会者管理モード",
    exitAdmin: "退出",
    totalVIP: "総求道者",
    jpGroup: "日本語圏",
    promising: "有望",
    listTitle: "登録名簿",
    latestOrder: "最新順",
    installTitle: "ホーム画面に追加",
    iosGuide: "iPhone (Safari)",
    iosStep1: "画面下の [共有] ボタンをタップ",
    iosStep2: "メニューを上げて [ホーム画面に追加] を選択",
    iosStep3: "右上の [追加] ボタンをタップして完了！",
    androidGuide: "Galaxy/Android (Chrome)",
    androidStep1: "画面右上の 3点リーダー(⋮) をタップ",
    androidStep2: "[アプリをインストール] または [ホーム画面に追加] を選択",
    androidStep3: "[インストール] または [追加] をタップして完了",
    adminPwTitle: "管理者パスワード (0000)",
    cancel: "キャンセル",
    confirm: "確認",

    // Admin Settings
    settingsTitle: "教会情報設定",
    churchNameKoLabel: "教会名 (韓国語)",
    churchNameJaLabel: "教会名 (日本語)",
    saveBtn: "保存する",
    saveSuccess: "保存されました。",
    editSubmissionTitle: "登録内容の修正",
    updateBtn: "修正完了",

    // [추가] 엑셀/메일 관련
    exportExcelBtn: "Excel(CSV)保存",
    sendMailBtn: "メールアプリを開く",
    exportSuccessMsg: "ファイルがダウンロードされました。\n'メールアプリを開く'を押してファイルを添付してください。"
  }
};

const FamilyEvangelismApp = () => {
  const [appLang, setAppLang] = useState('ko'); 
  const t = TRANSLATIONS[appLang]; 

  const ADMIN_PASSWORD = "0000"; 
  
  const [churchNameConfig, setChurchNameConfig] = useState({
    ko: "동경희망그리스도교회",
    ja: "東京希望キリスト教会"
  });

  const [step, setStep] = useState(0); 
  const [showInstallGuide, setShowInstallGuide] = useState(false);
  const [isAdminMode, setIsAdminMode] = useState(false);
  const [adminTab, setAdminTab] = useState('list'); 
  const [adminPasswordInput, setAdminPasswordInput] = useState('');
  const [showAdminLogin, setShowAdminLogin] = useState(false);
  
  const [editingSubmission, setEditingSubmission] = useState(null);

  const [tempChurchNameConfig, setTempChurchNameConfig] = useState({...churchNameConfig});

  const [submissions, setSubmissions] = useState([
    {
      id: 1, 
      submitterName: '이권사',
      name: '김철수', gender: '남성', nationality: '대한민국', language: '한국어',
      relationship: '남편', ageGroup: '40대', job: '직장인/회사원', livingType: '동거 (함께)', religion: '무교',
      favorability: 2, pastExperience: '없음', rejectionReason: '무관심',
      interests: ['사업/재정', '건강/질병'], invitationEvent: '구역 식사 모임', prayerRequest: '사업의 번창과 마음의 평안을 위해', commitment: '매일 1분 기도하기',
      note: '주말에는 시간이 있습니다.',
      date: '2024-05-20'
    },
    {
      id: 2, 
      submitterName: '박집사',
      name: '사토 아이코', gender: '여성', nationality: '일본', language: '일본어',
      relationship: '며느리', ageGroup: '30대', job: '주부', livingType: '인근 거주', religion: '신도교 (Shinto)',
      favorability: 4, pastExperience: '주일학교 출신', rejectionReason: '제사 및 문화적 이유',
      interests: ['자녀교육/진로', '가정불화'], invitationEvent: '음악회/바자회 등 문화행사', prayerRequest: '아이 양육 문제로 힘들어하는데 교회 문화교실에 연결되길', commitment: '올해 안에 교회 뜰 밟게 하기',
      note: '',
      date: '2024-05-21'
    }
  ]);

  const [formData, setFormData] = useState({
    submitterName: '',
    name: '',
    gender: '',
    nationality: '대한민국',
    language: '한국어',
    relationship: '',
    ageGroup: '',
    job: '',
    livingType: '',
    religion: '',
    favorability: 3,
    pastExperience: '',
    rejectionReason: '',
    interests: [],
    invitationEvent: '',
    prayerRequest: '',
    commitment: '',
    note: ''
  });

  const [langMessage, setLangMessage] = useState('');

  useEffect(() => {
    const isTargetJp = formData.language.includes('일본') || formData.language.includes('日本');
    if (isTargetJp) {
      setLangMessage(t.langMsg);
    } else {
      setLangMessage('');
    }
  }, [formData.language, appLang, t]);

  const handleChange = (field, value) => {
    setFormData(prev => ({ ...prev, [field]: value }));
  };

  const handleCheckboxChange = (value) => {
    setFormData(prev => {
      const newInterests = prev.interests.includes(value)
        ? prev.interests.filter(i => i !== value)
        : [...prev.interests, value];
      return { ...prev, interests: newInterests };
    });
  };

  const nextStep = () => setStep(prev => prev + 1);
  const prevStep = () => setStep(prev => prev - 1);
  const progress = step <= 4 ? (step / 5) * 100 : 100; 

  const handleSubmit = () => {
    const newData = {
      ...formData,
      id: submissions.length + 1,
      date: new Date().toLocaleDateString()
    };
    setSubmissions(prev => [newData, ...prev]);
    nextStep(); 
  };

  const handleAdminLogin = () => {
    if (adminPasswordInput === ADMIN_PASSWORD) {
      setIsAdminMode(true);
      setShowAdminLogin(false);
      setAdminPasswordInput('');
      setTempChurchNameConfig({...churchNameConfig});
    } else {
      alert("비밀번호가 일치하지 않습니다.\nパスワードが一致しません。");
    }
  };

  const saveChurchSettings = () => {
    setChurchNameConfig(tempChurchNameConfig);
    alert(t.saveSuccess);
  };

  const handleUpdateSubmission = () => {
    setSubmissions(prev => prev.map(sub => sub.id === editingSubmission.id ? editingSubmission : sub));
    setEditingSubmission(null);
    alert(t.saveSuccess);
  };

  // [추가] 엑셀(CSV) 다운로드 함수
  const exportToCSV = () => {
    // BOM 추가 (한글 깨짐 방지)
    const BOM = "\uFEFF";
    
    // CSV 헤더
    const headers = [
      "No", "작성자", "대상자", "성별", "국적", "언어", "관계", "연령", "직업", "거주", 
      "종교", "호감도", "신앙경험", "거부이유", "관심사", "초청계기", "기도제목", "다짐", "메모", "등록일"
    ];

    // 데이터 변환
    const csvRows = submissions.map((row, index) => [
      submissions.length - index, // 역순 번호 (최신이 1번인 경우) or index + 1
      `"${row.submitterName}"`,
      `"${row.name}"`,
      `"${row.gender}"`,
      `"${row.nationality}"`,
      `"${row.language}"`,
      `"${row.relationship}"`,
      `"${row.ageGroup}"`,
      `"${row.job || ''}"`,
      `"${row.livingType}"`,
      `"${row.religion}"`,
      row.favorability,
      `"${row.pastExperience}"`,
      `"${row.rejectionReason}"`,
      `"${(row.interests || []).join(', ')}"`,
      `"${row.invitationEvent}"`,
      `"${(row.prayerRequest || '').replace(/\n/g, ' ')}"`, // 줄바꿈 제거
      `"${row.commitment}"`,
      `"${(row.note || '').replace(/\n/g, ' ')}"`,
      `"${row.date}"`
    ]);

    // CSV 문자열 조합
    const csvString = BOM + [headers.join(','), ...csvRows.map(r => r.join(','))].join('\n');

    // 다운로드 링크 생성 및 클릭
    const blob = new Blob([csvString], { type: 'text/csv;charset=utf-8;' });
    const url = URL.createObjectURL(blob);
    const link = document.createElement('a');
    link.href = url;
    link.setAttribute('download', `태신자명단_${new Date().toISOString().slice(0,10)}.csv`);
    document.body.appendChild(link);
    link.click();
    document.body.removeChild(link);

    // 안내 메시지
    alert(t.exportSuccessMsg);
  };

  // [추가] 메일 앱 열기
  const openMailClient = () => {
    const subject = encodeURIComponent(`[${churchNameConfig[appLang]}] 태신자 명단 파일 첨부`);
    const body = encodeURIComponent("방금 다운로드된 엑셀(CSV) 파일을 이곳에 첨부해주세요.");
    window.location.href = `mailto:?subject=${subject}&body=${body}`;
  };

  // --- Components ---

  const Header = () => (
    <div className="bg-white/10 backdrop-blur-md p-4 sticky top-0 z-20 border-b border-white/10">
      <div className="max-w-md mx-auto">
        <div className="flex justify-center items-center mb-1">
          <h1 className="text-lg font-bold flex items-center gap-2 truncate text-white drop-shadow-md">
            <Heart className="w-5 h-5 fill-rose-400 text-rose-400 flex-shrink-0 animate-pulse" />
            <span className="truncate">{t.headerTitle}</span>
          </h1>
        </div>
        
        {/* Progress Bar */}
        {step > 0 && step < 6 && (
          <div className="w-full bg-black/20 h-1.5 mt-3 rounded-full overflow-hidden">
            <div 
              className="bg-yellow-400 h-full transition-all duration-500 ease-out shadow-[0_0_10px_rgba(250,204,21,0.5)]" 
              style={{ width: `${progress}%` }}
            ></div>
          </div>
        )}
      </div>
    </div>
  );

  const InstallGuideModal = () => {
    const userAgent = navigator.userAgent || navigator.vendor || window.opera;
    const isIOS = /iPad|iPhone|iPod/.test(userAgent) && !window.MSStream;
    const [activeTab, setActiveTab] = useState(isIOS ? 'ios' : 'android');

    return (
      <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/80 p-4 animate-fade-in backdrop-blur-sm">
        <div className="bg-white rounded-3xl max-w-sm w-full p-0 relative shadow-2xl overflow-hidden">
          <div className="bg-indigo-600 p-5 flex justify-between items-center text-white">
            <h3 className="text-lg font-bold flex items-center gap-2">
              <Download className="w-5 h-5" />
              {t.installTitle}
            </h3>
            <button onClick={() => setShowInstallGuide(false)} className="text-white/80 hover:text-white">
              <X className="w-6 h-6" />
            </button>
          </div>

          <div className="flex border-b border-gray-100">
            <button 
              onClick={() => setActiveTab('android')}
              className={`flex-1 py-3 text-sm font-bold flex items-center justify-center gap-2 ${activeTab === 'android' ? 'text-green-600 border-b-2 border-green-600 bg-green-50' : 'text-gray-400 hover:bg-gray-50'}`}
            >
              <Smartphone className="w-4 h-4" /> Android
            </button>
            <button 
              onClick={() => setActiveTab('ios')}
              className={`flex-1 py-3 text-sm font-bold flex items-center justify-center gap-2 ${activeTab === 'ios' ? 'text-blue-600 border-b-2 border-blue-600 bg-blue-50' : 'text-gray-400 hover:bg-gray-50'}`}
            >
              <Smartphone className="w-4 h-4" /> iPhone
            </button>
          </div>

          <div className="p-6">
            {activeTab === 'ios' ? (
              <div className="space-y-4 animate-fade-in">
                <div className="flex items-start gap-4">
                  <div className="bg-blue-100 text-blue-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">1</div>
                  <div>
                    <p className="text-gray-800 font-medium text-sm mb-1">{t.iosStep1}</p>
                    <div className="bg-gray-100 p-2 rounded flex justify-center"><Share className="w-5 h-5 text-blue-500" /></div>
                  </div>
                </div>
                <div className="w-0.5 h-6 bg-gray-200 ml-4"></div>
                <div className="flex items-start gap-4">
                  <div className="bg-blue-100 text-blue-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">2</div>
                  <div>
                    <p className="text-gray-800 font-medium text-sm mb-1">{t.iosStep2}</p>
                    <div className="bg-gray-100 p-2 rounded text-xs text-gray-500 font-bold border border-gray-300 inline-block">+ {t.installTitle}</div>
                  </div>
                </div>
                <div className="w-0.5 h-6 bg-gray-200 ml-4"></div>
                <div className="flex items-start gap-4">
                  <div className="bg-blue-100 text-blue-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">3</div>
                  <p className="text-gray-800 font-medium text-sm pt-1">{t.iosStep3}</p>
                </div>
              </div>
            ) : (
              <div className="space-y-4 animate-fade-in">
                <div className="flex items-start gap-4">
                  <div className="bg-green-100 text-green-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">1</div>
                  <div>
                    <p className="text-gray-800 font-medium text-sm mb-1">{t.androidStep1}</p>
                    <div className="bg-gray-100 p-2 rounded flex justify-center"><Menu className="w-5 h-5 text-gray-600" /></div>
                  </div>
                </div>
                <div className="w-0.5 h-6 bg-gray-200 ml-4"></div>
                <div className="flex items-start gap-4">
                  <div className="bg-green-100 text-green-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">2</div>
                  <div>
                    <p className="text-gray-800 font-medium text-sm mb-1">{t.androidStep2}</p>
                    <div className="bg-gray-100 p-2 rounded text-xs text-gray-500 font-bold border border-gray-300 inline-block flex items-center gap-1"><ArrowUpCircle className="w-3 h-3"/> {t.installTitle}</div>
                  </div>
                </div>
                <div className="w-0.5 h-6 bg-gray-200 ml-4"></div>
                <div className="flex items-start gap-4">
                  <div className="bg-green-100 text-green-600 w-8 h-8 rounded-full flex items-center justify-center font-bold flex-shrink-0">3</div>
                  <p className="text-gray-800 font-medium text-sm pt-1">{t.androidStep3}</p>
                </div>
              </div>
            )}
          </div>
          
          <div className="bg-gray-50 p-4 text-center">
            <button onClick={() => setShowInstallGuide(false)} className="w-full bg-indigo-600 text-white font-bold py-3 rounded-xl hover:bg-indigo-700 transition">
              {t.confirm}
            </button>
          </div>
        </div>
      </div>
    );
  };

  const EditSubmissionModal = () => {
    if (!editingSubmission) return null;
    return (
        <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/70 p-4 animate-fade-in backdrop-blur-sm">
            <div className="bg-white rounded-2xl w-full max-w-md p-6 relative shadow-2xl max-h-[90vh] overflow-y-auto">
                <button onClick={() => setEditingSubmission(null)} className="absolute top-4 right-4 text-gray-400 hover:text-gray-600">
                    <X className="w-6 h-6" />
                </button>
                <h3 className="text-lg font-bold text-gray-800 mb-4 flex items-center gap-2">
                    <Edit3 className="w-5 h-5 text-blue-600" /> {t.editSubmissionTitle}
                </h3>
                <div className="space-y-4">
                    <div>
                        <label className="text-xs font-bold text-gray-500 block mb-1">성도님 성함 / 作成者</label>
                        <input type="text" className="w-full p-2 border rounded" value={editingSubmission.submitterName} onChange={e => setEditingSubmission({...editingSubmission, submitterName: e.target.value})} />
                    </div>
                    <div>
                        <label className="text-xs font-bold text-gray-500 block mb-1">대상자 성명 / 対象者</label>
                        <input type="text" className="w-full p-2 border rounded" value={editingSubmission.name} onChange={e => setEditingSubmission({...editingSubmission, name: e.target.value})} />
                    </div>
                    <div>
                        <label className="text-xs font-bold text-gray-500 block mb-1">기도 제목 / 祈り</label>
                        <textarea className="w-full p-2 border rounded h-24" value={editingSubmission.prayerRequest} onChange={e => setEditingSubmission({...editingSubmission, prayerRequest: e.target.value})} />
                    </div>
                    <div>
                        <label className="text-xs font-bold text-gray-500 block mb-1">참고(메모) / メモ</label>
                        <textarea className="w-full p-2 border rounded h-20" value={editingSubmission.note || ''} onChange={e => setEditingSubmission({...editingSubmission, note: e.target.value})} />
                    </div>
                    <button onClick={handleUpdateSubmission} className="w-full bg-blue-600 text-white font-bold py-3 rounded-xl hover:bg-blue-700">{t.updateBtn}</button>
                </div>
            </div>
        </div>
    )
  }

  const AdminDashboard = () => {
    const totalCount = submissions.length;
    const jpCount = submissions.filter(s => s.language.includes('일본') || s.language.includes('日本')).length;
    const readyCount = submissions.filter(s => s.favorability >= 4).length;

    return (
      <div className="h-full bg-white/50 backdrop-blur-md flex flex-col animate-fade-in">
        <div className="bg-white/20 text-white p-4 sticky top-0 z-20 shadow-lg border-b border-white/10">
           {/* [수정] 관리자 헤더 레이아웃: 액션 버튼 배치 */}
           <div className="flex justify-between items-center mb-4">
            <h2 className="font-bold text-lg flex items-center gap-2 text-white">
                <Lock className="w-5 h-5 text-yellow-300" />
                {t.adminMode}
            </h2>
            <div className="flex gap-2">
                {/* [추가] 엑셀 내보내기 & 메일 버튼 */}
                {adminTab === 'list' && (
                    <>
                        <button onClick={exportToCSV} className="text-xs bg-green-600/80 hover:bg-green-600 px-3 py-1.5 rounded flex items-center gap-1 transition text-white border border-white/20">
                            <FileSpreadsheet className="w-3 h-3" /> <span className="hidden sm:inline">{t.exportExcelBtn}</span>
                        </button>
                        <button onClick={openMailClient} className="text-xs bg-blue-500/80 hover:bg-blue-500 px-3 py-1.5 rounded flex items-center gap-1 transition text-white border border-white/20">
                            <Mail className="w-3 h-3" /> <span className="hidden sm:inline">{t.sendMailBtn}</span>
                        </button>
                    </>
                )}
                <button onClick={() => setIsAdminMode(false)} className="text-xs bg-black/30 px-3 py-1.5 rounded hover:bg-black/50 flex items-center gap-1 transition text-white border border-white/20">
                    <LogOut className="w-3 h-3" />
                </button>
            </div>
           </div>
           
           <div className="flex bg-black/20 p-1 rounded-lg backdrop-blur-sm">
             <button 
                onClick={() => setAdminTab('list')}
                className={`flex-1 text-xs py-2 rounded font-bold flex items-center justify-center gap-1 transition ${adminTab === 'list' ? 'bg-white text-indigo-900 shadow-sm' : 'text-indigo-100 hover:text-white'}`}
             >
                <List className="w-3 h-3" /> {t.listTitle}
             </button>
             <button 
                onClick={() => setAdminTab('settings')}
                className={`flex-1 text-xs py-2 rounded font-bold flex items-center justify-center gap-1 transition ${adminTab === 'settings' ? 'bg-white text-indigo-900 shadow-sm' : 'text-indigo-100 hover:text-white'}`}
             >
                <Settings className="w-3 h-3" /> 설정/Setting
             </button>
           </div>
        </div>

        <div className="flex-1 overflow-y-auto p-4 space-y-6">
          {adminTab === 'list' ? (
            <>
              <div className="grid grid-cols-3 gap-3">
                <div className="bg-white/90 p-4 rounded-xl shadow-lg border border-white/50 text-center backdrop-blur-sm">
                  <div className="text-2xl font-bold text-blue-600">{totalCount}</div>
                  <div className="text-xs text-gray-500 font-medium">{t.totalVIP}</div>
                </div>
                <div className="bg-white/90 p-4 rounded-xl shadow-lg border border-white/50 text-center backdrop-blur-sm">
                  <div className="text-2xl font-bold text-red-500">{jpCount}</div>
                  <div className="text-xs text-gray-500 font-medium">{t.jpGroup}</div>
                </div>
                <div className="bg-white/90 p-4 rounded-xl shadow-lg border border-white/50 text-center backdrop-blur-sm">
                  <div className="text-2xl font-bold text-green-600">{readyCount}</div>
                  <div className="text-xs text-gray-500 font-medium">{t.promising}</div>
                </div>
              </div>

              <div className="bg-white/90 rounded-xl shadow-lg border border-white/50 overflow-hidden backdrop-blur-sm">
                <div className="bg-gray-50/50 p-3 border-b border-gray-200 font-bold text-gray-700 flex justify-between items-center">
                  <span className="flex items-center gap-2"><List className="w-4 h-4"/> {t.listTitle}</span>
                  <span className="text-xs font-normal text-gray-500 bg-white px-2 py-0.5 rounded border border-gray-200">{t.latestOrder}</span>
                </div>
                <div className="divide-y divide-gray-100">
                  {submissions.map((item) => (
                    <div key={item.id} onClick={() => setEditingSubmission(item)} className="p-4 hover:bg-indigo-50 transition cursor-pointer group">
                      <div className="flex justify-between items-start mb-1">
                        <span className="font-bold text-gray-800 flex items-center gap-2 group-hover:text-indigo-600 transition">
                          {item.name} 
                          <span className={`text-[10px] px-1.5 py-0.5 rounded ${item.gender === '남성' ? 'bg-blue-100 text-blue-600' : 'bg-pink-100 text-pink-600'}`}>{item.gender}</span>
                        </span>
                        <span className={`text-xs px-2 py-1 rounded-full font-medium ${item.favorability >= 4 ? 'bg-green-100 text-green-700' : 'bg-gray-100 text-gray-600'}`}>
                          ★ {item.favorability}
                        </span>
                      </div>
                      <div className="text-sm text-gray-600 mb-2">
                        {item.ageGroup} · {item.relationship} · {item.job ? item.job : '직업 미입력'} · {item.language}
                      </div>
                      <div className="text-xs text-indigo-800 bg-indigo-50 px-2 py-1 mb-2 rounded inline-block border border-indigo-100">
                        By: {item.submitterName}
                      </div>
                      <div className="text-xs bg-gray-50 p-3 rounded-lg text-gray-600 border border-gray-200 mt-2 italic">
                        <span className="font-bold text-gray-700 block mb-1 not-italic">Pray:</span> "{item.prayerRequest}"
                      </div>
                      {item.note && (
                          <div className="text-xs text-gray-500 mt-1 pl-1 border-l-2 border-gray-300">
                              Memo: {item.note}
                          </div>
                      )}
                    </div>
                  ))}
                </div>
              </div>
            </>
          ) : (
            <div className="bg-white/90 rounded-xl shadow-lg border border-white/50 p-6 space-y-6 backdrop-blur-sm">
                <h3 className="font-bold text-lg text-gray-800 flex items-center gap-2 border-b border-gray-200 pb-2">
                    <Edit3 className="w-5 h-5 text-indigo-600" />
                    {t.settingsTitle}
                </h3>
                
                <div className="space-y-4">
                    <div className="space-y-1">
                        <label className="text-sm font-bold text-gray-600">{t.churchNameKoLabel}</label>
                        <input 
                            type="text"
                            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                            value={tempChurchNameConfig.ko}
                            onChange={(e) => setTempChurchNameConfig({...tempChurchNameConfig, ko: e.target.value})}
                        />
                    </div>
                    <div className="space-y-1">
                        <label className="text-sm font-bold text-gray-600">{t.churchNameJaLabel}</label>
                        <input 
                            type="text"
                            className="w-full p-3 border border-gray-300 rounded-lg focus:ring-2 focus:ring-indigo-500 outline-none"
                            value={tempChurchNameConfig.ja}
                            onChange={(e) => setTempChurchNameConfig({...tempChurchNameConfig, ja: e.target.value})}
                        />
                    </div>
                </div>

                <button 
                    onClick={saveChurchSettings}
                    className="w-full bg-indigo-600 text-white font-bold py-3 rounded-xl hover:bg-indigo-700 transition shadow-md"
                >
                    {t.saveBtn}
                </button>
            </div>
          )}
        </div>
        <EditSubmissionModal />
      </div>
    );
  };

  // ... (이후 IntroStep 등 기존 컴포넌트 동일)
  const IntroStep = () => (
    <div className="flex flex-col items-center justify-center h-full text-center p-6 pb-10">
      
      <div className="flex-1 flex flex-col items-center justify-center w-full max-w-sm z-10">
        
        {/* Large Language Switcher - First Page Position */}
        <div className="w-full bg-white/20 backdrop-blur-md p-2 rounded-2xl flex gap-2 mb-8 border border-white/20 shadow-lg">
            <button 
                onClick={() => setAppLang('ko')}
                className={`flex-1 py-3 rounded-xl font-bold flex items-center justify-center gap-2 transition-all ${appLang === 'ko' ? 'bg-white text-indigo-700 shadow-xl scale-105' : 'text-white/80 hover:bg-white/10 hover:text-white'}`}
            >
                <span className="text-xl">🇰🇷</span> 한국어
            </button>
            <button 
                onClick={() => setAppLang('ja')}
                className={`flex-1 py-3 rounded-xl font-bold flex items-center justify-center gap-2 transition-all ${appLang === 'ja' ? 'bg-white text-indigo-700 shadow-xl scale-105' : 'text-white/80 hover:bg-white/10 hover:text-white'}`}
            >
                <span className="text-xl">🇯🇵</span> 日本語
            </button>
        </div>

        <div className="bg-white/95 backdrop-blur-xl p-8 rounded-3xl shadow-2xl w-full border border-white/50 relative overflow-hidden transform hover:scale-[1.01] transition-transform duration-300">
            <div className="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-pink-500 via-purple-500 to-indigo-500"></div>
            
            <div className="mb-8">
                {/* [수정] 교회 이름 폰트 강화 */}
                <h3 className="text-indigo-800 font-black text-lg tracking-widest uppercase mb-3 border-b-2 border-indigo-100 pb-2 inline-block drop-shadow-sm">{churchNameConfig[appLang]}</h3>
                {/* [수정] 제목 크기 및 굵기 강화 */}
                <h2 className="text-4xl font-black text-gray-900 leading-tight drop-shadow-sm">{t.introTitle}</h2>
            </div>
            
            <div className="text-gray-900 text-sm leading-loose mb-8 font-bold bg-indigo-50/70 p-6 rounded-2xl border border-indigo-200 shadow-sm">
                <p className="mb-6 text-lg text-indigo-900 whitespace-pre-line leading-relaxed font-black">
                    <span className="text-2xl block mb-2">✨</span>
                    {t.introDesc1}
                </p>
                <div className="w-12 h-1 bg-indigo-300/50 mx-auto mb-6 rounded-full"></div>
                <p className="text-gray-800 whitespace-pre-line leading-relaxed font-bold text-base">
                    {t.introDesc2}
                </p>
            </div>

            <button 
                onClick={nextStep}
                className="w-full bg-gradient-to-r from-indigo-600 to-purple-600 hover:from-indigo-700 hover:to-purple-700 text-white font-bold py-4 rounded-xl shadow-lg shadow-indigo-200 transition transform active:scale-[0.98] flex items-center justify-center gap-2 group text-lg"
            >
                {t.startBtn} <ChevronRight className="w-6 h-6 group-hover:translate-x-1 transition-transform" />
            </button>
        </div>

        <div className="mt-6 text-white/90 text-xs font-medium bg-white/10 p-4 rounded-xl backdrop-blur-md border border-white/20 shadow-sm leading-relaxed">
            {t.notice}
        </div>

        <div onClick={() => setShowInstallGuide(true)} className="mt-6 cursor-pointer text-xs text-white/90 underline hover:text-white transition font-bold flex items-center gap-1 bg-black/20 px-4 py-2 rounded-full hover:bg-black/30">
            <Download className="w-3 h-3" /> {t.installGuideLink}
        </div>
      </div>

      <div className="w-full flex justify-center mt-8 z-10">
        <button 
            onClick={() => setShowAdminLogin(true)}
            className="text-[10px] text-white/40 hover:text-white p-2 transition font-medium tracking-wider"
        >
            {t.adminBtn}
        </button>
      </div>
    </div>
  );

  const BasicInfoStep = () => (
    <div className="space-y-6 animate-fade-in p-2 min-h-full">
      <div className="bg-white/90 backdrop-blur-xl p-6 rounded-2xl border border-white/50 shadow-xl mb-6">
        <h3 className="text-xl font-extrabold text-indigo-900 border-b-2 border-indigo-100 pb-3 mb-4 flex items-center gap-2">
            <User className="w-6 h-6 text-indigo-600" />
            {t.step1Title}
        </h3>
        
        <div className="bg-indigo-50 p-5 rounded-2xl border border-indigo-200 mb-6 relative overflow-hidden shadow-inner">
            <label className="text-sm font-bold text-indigo-800 flex items-center gap-2 mb-2">
                {t.labelSubmitter}
            </label>
            <input 
            type="text" 
            placeholder={t.phSubmitter}
            className="w-full p-4 border border-indigo-200 rounded-xl focus:ring-2 focus:ring-indigo-500 outline-none bg-white shadow-sm text-lg text-gray-800" 
            value={formData.submitterName} 
            onChange={(e) => handleChange('submitterName', e.target.value)} 
            />
            <p className="text-xs text-indigo-600 flex items-center gap-1 mt-2 font-medium"><CheckCircle className="w-3 h-3"/> {t.descSubmitter}</p>
        </div>

        <div className="space-y-6">
            <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700">{t.labelTarget}</label>
                <input type="text" placeholder={t.phTarget} className="w-full p-4 border border-gray-300 rounded-xl focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm text-lg" value={formData.name} onChange={(e) => handleChange('name', e.target.value)} />
            </div>
            
            <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700">{t.labelGender}</label>
                <div className="flex gap-3">
                {t.optionsGender.map((g) => (
                    <label key={g} className={`flex-1 p-3 rounded-xl border-2 cursor-pointer text-center transition font-bold shadow-sm ${formData.gender === g ? 'bg-indigo-600 border-indigo-600 text-white' : 'bg-white border-gray-200 text-gray-500 hover:bg-gray-50'}`}>
                    <input type="radio" name="gender" value={g} checked={formData.gender === g} onChange={() => handleChange('gender', g)} className="hidden"/>{g}
                    </label>
                ))}
                </div>
            </div>

            <div className="grid grid-cols-2 gap-4">
                <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700 flex items-center gap-1"><Globe className="w-3 h-3 text-indigo-500"/> {t.labelNation}</label>
                <select className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.nationality} onChange={(e) => handleChange('nationality', e.target.value)}>
                    {t.optionsNation.map(o => <option key={o}>{o}</option>)}
                </select>
                </div>
                <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700 flex items-center gap-1"><Mic className="w-3 h-3 text-indigo-500"/> {t.labelLang}</label>
                <select className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.language} onChange={(e) => handleChange('language', e.target.value)}>
                    {t.optionsLang.map(o => <option key={o}>{o}</option>)}
                </select>
                </div>
            </div>
            {langMessage && <div className="text-xs text-indigo-800 bg-indigo-100 p-3 rounded-lg border border-indigo-200 shadow-sm flex items-center gap-2 font-medium"><Sparkles className="w-4 h-4 text-indigo-500"/> {langMessage}</div>}
            
            <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700 flex items-center gap-1"><Briefcase className="w-3 h-3 text-indigo-500"/> {t.labelJob}</label>
                <select 
                    className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" 
                    value={formData.job} 
                    onChange={(e) => handleChange('job', e.target.value)}
                >
                    <option value="">{t.selectPlaceholder}</option>
                    {t.optionsJob.map(job => <option key={job}>{job}</option>)}
                </select>
            </div>

            <div className="grid grid-cols-2 gap-4">
                <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700">{t.labelRel}</label>
                <select className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.relationship} onChange={(e) => handleChange('relationship', e.target.value)}>
                    <option value="">{t.selectPlaceholder}</option>
                    {t.optionsRel.map(o => <option key={o}>{o}</option>)}
                </select>
                </div>
                <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700">{t.labelAge}</label>
                <select className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.ageGroup} onChange={(e) => handleChange('ageGroup', e.target.value)}>
                    <option value="">{t.selectPlaceholder}</option>
                    {t.optionsAge.map(o => <option key={o}>{o}</option>)}
                </select>
                </div>
            </div>

            <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700 flex items-center gap-1"><Home className="w-3 h-3 text-indigo-500"/> {t.labelLive}</label>
                <div className="grid grid-cols-2 gap-3">
                {t.optionsLive.map((live) => (
                    <label key={live} className={`p-3 text-sm rounded-xl border-2 cursor-pointer text-center transition font-medium shadow-sm ${formData.livingType === live ? 'bg-green-50 border-green-500 text-green-700' : 'bg-white border-gray-200 text-gray-500 hover:bg-gray-50'}`}>
                    <input type="radio" name="livingType" value={live} checked={formData.livingType === live} onChange={() => handleChange('livingType', live)} className="hidden"/>{live}
                </label>
                ))}
                </div>
            </div>

            <div className="space-y-2">
                <label className="text-sm font-bold text-gray-700">{t.labelReligion}</label>
                <select className="w-full p-3 border border-gray-300 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.religion} onChange={(e) => handleChange('religion', e.target.value)}>
                <option value="">{t.selectPlaceholder}</option>
                {t.optionsReligion.map(o => <option key={o}>{o}</option>)}
                </select>
            </div>
        </div>
      </div>
    </div>
  );

  const SpiritualStep = () => (
    <div className="space-y-6 animate-fade-in p-2 min-h-full">
      <div className="bg-white/90 backdrop-blur-xl p-6 rounded-2xl border border-white/50 shadow-xl">
        <h3 className="text-xl font-extrabold text-indigo-900 border-b-2 border-indigo-100 pb-3 mb-6 flex items-center gap-2">
            <Heart className="w-6 h-6 text-indigo-600" />
            {t.step2Title}
        </h3>
        
        <div className="space-y-4 mb-8">
            <label className="text-sm font-bold text-indigo-900">{t.labelFavor}</label>
            <div className="bg-indigo-50 p-5 rounded-2xl border border-indigo-200 shadow-inner">
                <div className="flex justify-between items-center mb-2">
                {[1, 2, 3, 4, 5].map((score) => (
                    <button key={score} onClick={() => handleChange('favorability', score)} className={`flex flex-col items-center gap-2 transition transform ${formData.favorability === score ? 'scale-110' : 'opacity-60 hover:opacity-100'}`}>
                    <Star className={`w-10 h-10 ${formData.favorability >= score ? 'fill-amber-400 text-amber-400 drop-shadow-sm' : 'text-gray-300'}`} />
                    <span className={`text-sm font-bold ${formData.favorability >= score ? 'text-indigo-800' : 'text-gray-400'}`}>{score}</span>
                    </button>
                ))}
                </div>
                <div className="flex justify-between text-xs text-indigo-500 px-2 font-bold"><span>{t.favorLabels[0]}</span><span>{t.favorLabels[1]}</span></div>
            </div>
        </div>

        <div className="space-y-6">
            <div className="space-y-3">
                <label className="text-sm font-bold text-indigo-900">{t.labelExp}</label>
                <div className="flex flex-col gap-3">
                {t.optionsExp.map((exp) => (
                    <label key={exp} className={`flex items-center p-4 rounded-xl border-2 cursor-pointer transition shadow-sm ${formData.pastExperience === exp ? 'bg-white border-indigo-500 text-indigo-700 font-bold' : 'bg-white/50 border-indigo-100 text-gray-600 hover:bg-white'}`}>
                    <input type="radio" name="pastExperience" value={exp} checked={formData.pastExperience === exp} onChange={() => handleChange('pastExperience', exp)} className="mr-3 accent-indigo-600 w-5 h-5"/><span className="text-sm">{exp}</span>
                    </label>
                ))}
                </div>
            </div>

            <div className="space-y-3">
                <label className="text-sm font-bold text-indigo-900">{t.labelReject}</label>
                <select className="w-full p-4 border border-indigo-200 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.rejectionReason} onChange={(e) => handleChange('rejectionReason', e.target.value)}>
                <option value="">{t.selectPlaceholder}</option>
                {t.optionsReject.map(o => <option key={o}>{o}</option>)}
                </select>
            </div>
        </div>
      </div>
    </div>
  );

  const ContactStep = () => (
    <div className="space-y-6 animate-fade-in p-2 min-h-full">
      <div className="bg-white/90 backdrop-blur-xl p-6 rounded-2xl border border-white/50 shadow-xl">
        <h3 className="text-xl font-extrabold text-indigo-900 border-b-2 border-indigo-100 pb-3 mb-6 flex items-center gap-2">
            <MessageCircle className="w-6 h-6 text-indigo-600" />
            {t.step3Title}
        </h3>
        
        <div className="space-y-3 mb-8">
            <label className="text-sm font-bold text-indigo-900 flex items-center gap-2"><HandHeart className="w-4 h-4"/> {t.labelInterest}</label>
            <div className="grid grid-cols-2 gap-3">
            {t.optionsInterest.map((item) => (
                <button key={item} onClick={() => handleCheckboxChange(item)} className={`p-3 text-sm rounded-xl border-2 text-left transition shadow-sm ${formData.interests.includes(item) ? 'bg-indigo-500 border-indigo-500 text-white font-bold' : 'bg-white border-indigo-100 text-gray-600 hover:bg-indigo-50'}`}>
                {formData.interests.includes(item) && <Check className="w-4 h-4 inline mr-1"/>}{item}
                </button>
            ))}
            </div>
        </div>

        <div className="space-y-3">
            <label className="text-sm font-bold text-indigo-900 flex items-center gap-2"><Sparkles className="w-4 h-4"/> {t.labelInvite}</label>
            <select className="w-full p-4 border border-indigo-200 rounded-xl bg-white focus:ring-2 focus:ring-indigo-500 outline-none shadow-sm" value={formData.invitationEvent} onChange={(e) => handleChange('invitationEvent', e.target.value)}>
            <option value="">{t.selectPlaceholder}</option>
            {t.optionsInvite.map(o => <option key={o}>{o}</option>)}
            </select>
        </div>
      </div>
    </div>
  );

  const PrayerStep = () => (
    <div className="space-y-6 animate-fade-in p-2 min-h-full">
      <div className="bg-white/90 backdrop-blur-xl p-6 rounded-2xl border border-white/50 shadow-xl">
        <h3 className="text-xl font-extrabold text-indigo-900 border-b-2 border-indigo-100 pb-3 mb-6 flex items-center gap-2">
            <Briefcase className="w-6 h-6 text-indigo-600" />
            {t.step4Title}
        </h3>
        
        <div className="space-y-3 mb-8">
            <label className="text-sm font-bold text-indigo-900">{t.labelPrayer}</label>
            <textarea 
                placeholder={t.phPrayer} 
                className="w-full p-4 border border-indigo-200 rounded-xl h-32 resize-none focus:ring-2 focus:ring-indigo-500 outline-none leading-relaxed shadow-inner bg-indigo-50/50" 
                value={formData.prayerRequest} 
                onChange={(e) => handleChange('prayerRequest', e.target.value)}
            />
        </div>

        {/* 참고 메모 */}
        <div className="space-y-3 mb-8">
            <label className="text-sm font-bold text-indigo-900 flex items-center gap-1"><FileText className="w-4 h-4"/> {t.labelNote}</label>
            <textarea 
                placeholder={t.phNote} 
                className="w-full p-4 border border-indigo-200 rounded-xl h-24 resize-none focus:ring-2 focus:ring-indigo-500 outline-none leading-relaxed shadow-inner bg-white/80" 
                value={formData.note} 
                onChange={(e) => handleChange('note', e.target.value)}
            />
        </div>

        <div className="space-y-3">
            <label className="text-sm font-bold text-indigo-900">{t.labelCommit}</label>
            <div className="bg-indigo-50 p-2 rounded-xl border border-indigo-100 shadow-inner space-y-1">
            {t.optionsCommit.map((commit) => (
                <label key={commit} className="flex items-center space-x-3 cursor-pointer p-4 rounded-lg hover:bg-white/50 transition bg-white/30 mb-1 last:mb-0">
                <input type="radio" name="commitment" value={commit} checked={formData.commitment === commit} onChange={() => handleChange('commitment', commit)} className="w-5 h-5 accent-indigo-600"/><span className="text-gray-700 text-sm font-bold">{commit}</span>
                </label>
            ))}
            </div>
        </div>
      </div>
    </div>
  );

  // 검토 단계 (Step 5)
  const ReviewStep = () => {
    return (
        <div className="space-y-6 animate-fade-in p-2 min-h-full">
            <div className="bg-white/95 backdrop-blur-xl p-6 rounded-2xl border border-white/50 shadow-2xl">
                <h3 className="text-xl font-extrabold text-slate-800 border-b-2 border-slate-200 pb-3 mb-4 flex items-center gap-2">
                    <CheckCircle className="w-6 h-6 text-slate-600" />
                    {t.step5Title}
                </h3>
                <p className="text-sm text-gray-500 mb-6 whitespace-pre-line">{t.reviewNotice}</p>

                <div className="space-y-4">
                    {/* Basic Info Summary */}
                    <div className="bg-indigo-50 p-4 rounded-xl border border-indigo-100 relative">
                        <button onClick={() => setStep(1)} className="absolute top-2 right-2 text-xs bg-white text-indigo-600 px-3 py-1 rounded-full border border-indigo-200 font-bold hover:bg-indigo-100 shadow-sm">
                            {t.editBtn}
                        </button>
                        <h4 className="font-bold text-indigo-900 text-sm mb-2 flex items-center gap-1"><User className="w-3 h-3"/> {t.step1Title}</h4>
                        <div className="text-sm text-gray-700 space-y-1 pl-1">
                            <p><span className="text-gray-500 font-medium">대상자:</span> {formData.name} <span className="text-xs text-gray-400">({formData.gender})</span></p>
                            <p><span className="text-gray-500 font-medium">관계:</span> {formData.relationship}</p>
                            <p><span className="text-gray-500 font-medium">언어:</span> {formData.language}</p>
                        </div>
                    </div>

                    {/* Spiritual Summary */}
                    <div className="bg-indigo-50 p-4 rounded-xl border border-indigo-100 relative">
                        <button onClick={() => setStep(2)} className="absolute top-2 right-2 text-xs bg-white text-indigo-600 px-3 py-1 rounded-full border border-indigo-200 font-bold hover:bg-indigo-100 shadow-sm">
                            {t.editBtn}
                        </button>
                        <h4 className="font-bold text-indigo-900 text-sm mb-2 flex items-center gap-1"><Heart className="w-3 h-3"/> {t.step2Title}</h4>
                        <div className="text-sm text-gray-700 space-y-1 pl-1">
                            <p><span className="text-gray-500 font-medium">호감도:</span> {formData.favorability}점</p>
                            <p><span className="text-gray-500 font-medium">종교:</span> {formData.religion}</p>
                        </div>
                    </div>

                    {/* Prayer Summary */}
                    <div className="bg-indigo-50 p-4 rounded-xl border border-indigo-100 relative">
                        <button onClick={() => setStep(4)} className="absolute top-2 right-2 text-xs bg-white text-indigo-600 px-3 py-1 rounded-full border border-indigo-200 font-bold hover:bg-indigo-100 shadow-sm">
                            {t.editBtn}
                        </button>
                        <h4 className="font-bold text-indigo-900 text-sm mb-2 flex items-center gap-1"><Briefcase className="w-3 h-3"/> {t.step4Title}</h4>
                        <div className="text-sm text-gray-700 space-y-1 pl-1">
                            <p className="line-clamp-2 italic text-gray-600">"{formData.prayerRequest}"</p>
                            {formData.note && <p className="text-xs text-gray-500 mt-2 border-t border-indigo-200 pt-2 flex gap-1"><FileText className="w-3 h-3"/> {formData.note}</p>}
                        </div>
                    </div>
                </div>
            </div>
        </div>
    )
  }

  const CompleteStep = () => (
    <div className="flex flex-col items-center justify-center h-full text-center space-y-6 p-6 animate-fade-in">
      <div className="bg-white/90 p-8 rounded-full mb-4 shadow-2xl border-4 border-green-200 animate-bounce backdrop-blur-sm">
        <Check className="w-20 h-20 text-green-500" />
      </div>
      <h2 className="text-3xl font-black text-white drop-shadow-md tracking-tight">{t.submitCompleteTitle}</h2>
      
      <div className="bg-white/95 p-8 rounded-3xl shadow-2xl border border-white/50 w-full max-w-sm relative overflow-hidden backdrop-blur-xl">
        <div className="absolute top-0 left-0 w-full h-2 bg-gradient-to-r from-green-400 to-emerald-500"></div>
        <Quote className="absolute top-4 right-4 w-8 h-8 text-gray-100 rotate-180" />
        <p className="text-gray-700 mb-6 whitespace-pre-line leading-loose text-lg font-medium">
          <span className="font-bold text-indigo-600 text-xl">{formData.submitterName}</span> {appLang === 'ko' ? '성도님' : ''}<br/>
          <span className="font-bold text-indigo-600 text-xl">{formData.name}</span> {t.submitCompleteDesc}
        </p>
        <div className="border-t border-gray-100 pt-4 flex justify-between items-center text-xs text-gray-400">
            <span>{churchNameConfig[appLang]}</span>
            <span>{new Date().toLocaleDateString()}</span>
        </div>
      </div>
      
      <div className="bg-white/20 p-6 rounded-xl border border-white/30 max-w-sm shadow-lg backdrop-blur-sm">
        <p className="text-sm text-white italic font-serif font-medium leading-relaxed drop-shadow">
            {t.verse}
        </p>
      </div>

      <button 
        onClick={() => {
            setStep(0);
            setFormData({
                submitterName: '', name: '', gender: '', nationality: '대한민국', language: '한국어', relationship: '', ageGroup: '', job: '', livingType: '', religion: '', favorability: 3, pastExperience: '', rejectionReason: '', interests: [], invitationEvent: '', prayerRequest: '', commitment: '', note: ''
            });
        }}
        className="w-full bg-white text-indigo-900 font-bold py-4 rounded-2xl mt-4 shadow-xl hover:bg-indigo-50 transition transform active:scale-95 flex items-center justify-center gap-2"
      >
        <RefreshCw className="w-4 h-4" /> {t.anotherFamilyBtn}
      </button>
    </div>
  );

  return (
    <div className="min-h-screen font-sans bg-gradient-to-br from-violet-600 via-indigo-700 to-purple-800 animate-gradient flex items-center justify-center">
      {/* Global Background Container */}
      <div className="w-full max-w-md h-[850px] shadow-2xl rounded-none md:rounded-3xl overflow-hidden flex flex-col relative border-x border-white/10 bg-transparent">
        
        {isAdminMode ? (
          <AdminDashboard />
        ) : (
          <>
            {step > 0 && <Header />}
            {/* Main Content Area - Transparent to show global gradient */}
            <div className="flex-1 overflow-y-auto p-4 scrollbar-hide">
              {step === 0 && <IntroStep />}
              {step === 1 && <BasicInfoStep />}
              {step === 2 && <SpiritualStep />}
              {step === 3 && <ContactStep />}
              {step === 4 && <PrayerStep />}
              {step === 5 && <ReviewStep />}
              {step === 6 && <CompleteStep />}
            </div>
            
            {showInstallGuide && <InstallGuideModal />}
            {showAdminLogin && (
              <div className="fixed inset-0 z-50 flex items-center justify-center bg-black/60 p-4 backdrop-blur-sm">
                <div className="bg-white rounded-2xl p-8 w-full max-w-xs text-center shadow-2xl">
                  <h3 className="font-bold text-gray-800 mb-6 text-lg">{t.adminPwTitle}</h3>
                  <input 
                    type="password" 
                    inputMode="numeric" 
                    maxLength={4} 
                    className="w-full p-3 border rounded-xl mb-6 text-center text-2xl tracking-[0.5em] font-bold focus:ring-2 focus:ring-blue-500 outline-none" 
                    value={adminPasswordInput}
                    onChange={(e) => setAdminPasswordInput(e.target.value)}
                    placeholder="••••"
                  />
                  <div className="flex gap-3">
                    <button onClick={() => setShowAdminLogin(false)} className="flex-1 py-3 bg-gray-100 text-gray-600 rounded-xl font-bold hover:bg-gray-200 transition">{t.cancel}</button>
                    <button onClick={handleAdminLogin} className="flex-1 py-3 bg-blue-600 text-white rounded-xl font-bold hover:bg-blue-700 transition shadow-md">{t.confirm}</button>
                  </div>
                </div>
              </div>
            )}

            {step > 0 && step < 6 && (
              <div className="p-4 bg-white/10 backdrop-blur-md border-t border-white/10 flex justify-between items-center shadow-lg sticky bottom-0 z-10">
                <button onClick={prevStep} className="px-6 py-3 rounded-xl text-white/80 hover:bg-white/10 font-bold flex items-center gap-1 transition">
                  <ChevronLeft className="w-5 h-5" /> {t.prevBtn}
                </button>
                <button 
                  onClick={step === 5 ? handleSubmit : nextStep} 
                  className={`px-8 py-3 rounded-xl text-indigo-900 font-bold shadow-lg transition transform active:scale-95 flex items-center gap-2 ${step === 5 ? 'bg-gradient-to-r from-green-400 to-emerald-400 hover:from-green-500 hover:to-emerald-500' : 'bg-white hover:bg-indigo-50'}`}
                >
                  {step === 5 ? t.submitBtn : (step === 4 ? t.reviewBtn : t.nextBtn)} {step !== 5 && <ChevronRight className="w-5 h-5" />}
                </button>
              </div>
            )}
          </>
        )}
      </div>
      <style>{`
        .scrollbar-hide::-webkit-scrollbar { display: none; }
        .scrollbar-hide { -ms-overflow-style: none; scrollbar-width: none; }
        @keyframes fade-in { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }
        .animate-fade-in { animation: fade-in 0.6s cubic-bezier(0.16, 1, 0.3, 1) forwards; }
        
        @keyframes gradient {
            0% { background-position: 0% 50%; }
            50% { background-position: 100% 50%; }
            100% { background-position: 0% 50%; }
        }
        .animate-gradient {
            background-size: 200% 200%;
            animation: gradient 10s ease infinite;
        }
      `}</style>
    </div>
  );
};

export default FamilyEvangelismApp;
