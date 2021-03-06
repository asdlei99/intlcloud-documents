### 원본 서버
원본 서버란 사용자가 안정적으로 실행하는 비즈니스 서버를 의미하며, Tencent Cloud CDN의 원본 서버는 외부 원본 서버 혹은 객체 스토리지(COS) 중에서 선택할 수 있습니다.

### 외부 원본 서버
클라이언트 자체의 웹서비스가 속한 서버를 가리키며, 가속 도메인에 액세스할 시 서버와 대응하는 외부 네트워크의 IP 주소를 원본 서버로 입력 가능합니다.

### COS 원본
리소스가 Tencent Cloud 객체 스토리지(COS)에 저장되어 있다면, 바로 특정 bucket을 원본 서버로 선택할 수 있습니다.

### 엣지 노드
리전이 다른 사용자의 요청에 신속히 응답하기 위해, Tencent Cloud CDN이 클라이언트의 원본 서버 콘텐츠 캐싱에 사용하는 네트워크 노드입니다.

### CNAME 기록
CNAME 기록이란 도메인 리졸브 과정에서의 별명 레코드(Canonical Name)를 말합니다.
예를 들어 이름이 'host.example.com'인 서버가 WWW와 MAIL 서비스를 동시에 제공함으로써 사용자는 편리하게 서비스에 액세스할 수 있습니다. 이 서버는 DNS 리졸브 서비스 제공 업체에서 각각 'www.example.com'과 'mail.example.com'의 CNAME 두 개를 추가할 수 있고, 이 두 CNAME에 대한 모든 액세스 요청은 'host.example.com'으로 전달됩니다.

### CNAME 도메인
CNAME 도메인이란 Tencent Cloud CDN 콘솔에서 가속 도메인에 액세스한 뒤, 시스템에서 해당하는 도메인에게 '.cdn.dnsv1.com'을 접미사로 할당하는 도메인입니다. 사용자가 도메인 서비스 제공 업체에서 설정한 CNAME 기록이 적용된 후부터 리졸브 작업이 Tencent Cloud CDN에 정식으로 전달되며, 해당 도메인의 모든 요청은 Tencent Cloud CDN의 엣지 노드로 전달됩니다.

### 정적 콘텐츠
사용자가 특정 리소스에 여러 차례 액세스할 때, 응답한 데이터가 모두 동일한 콘텐츠임을 의미합니다.
예시: html, css 및 js 파일, 이미지, 비디오, 소프트웨어 설치 패키지, apk 파일, 압축 파일 등

### 동적 콘텐츠
사용자가 특정 리소스에 여러 차례 액세스할 때, 응답한 데이터가 동일한 콘텐츠가 아님을 의미합니다.
예시: API 인터페이스, .jsp, .asp, .php, .perl 및 .cgi 파일 등

### Origin-pull
CDN에서의 Origin-pull이란 사용자가 브라우저를 통해 요청을 전송할 때, 해당 요청을 각 노드의 캐시 서버가 아닌 원본 서버에서 응답하는 것을 말합니다. 일반적으로 CDN 노드의 캐시 서버에 응답할 콘텐츠의 캐시가 없거나, 응답할 콘텐츠가 원본 서버에서 수정된 경우 원본 서버로부터 불러옵니다.

### 호스트 헤더
CDN 노드가 Origin-pull 할 때 원본 서버에서 액세스한 사이트 도메인, 즉 Origin-pull 도메인으로, 자세한 내용은 [Origin-pull 도메인 설정](https://intl.cloud.tencent.com/document/product/228/6289)을 참조 바랍니다.

### 도메인
도메인(Domain Name)은 사람들이 기억하고 사용하기 쉽도록 만든 서버 주소로 웹 사이트, 이메일, FTP 등이 포함됩니다. 데이터 전송 시 컴퓨터의 전자 방위(혹은 지리적 위치)를 표시하는 데 사용합니다.
