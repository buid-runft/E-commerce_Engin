📘 เอกสารการใช้งานและพัฒนาระบบ E-commerce Code Generator

📖 ส่วนที่ 1: เอกสารการใช้งาน (User Manual)

🎯 1.1 ภาพรวมระบบ (System Overview)

```
┌─────────────────────────────────────────────────────────────┐
│                  E-COMMERCE CODE GENERATOR                   │
├─────────────────────────────────────────────────────────────┤
│  Input:  "31.01.04.262143" (Bitmask Code)                   │
│          ↓                                                   │
│  Process: Layer Warp Engine + Business Logic Injection      │
│          ↓                                                   │
│  Output: 24 Layers ของโมดูล VendorProduct พร้อมใช้งาน      │
└─────────────────────────────────────────────────────────────┘
```

📋 1.2 รูปแบบการเรียกใช้ (Usage Patterns)

รูปแบบพื้นฐาน:

```bash
# สร้างโมดูลเดียว
php artisan generate:module <module>.<layer>.<behavior>.<features> [options]

# สร้างทั้งหมด 52 โมดูล
php artisan generate:all [options]

# ถอดรหัส Bitmask
php artisan decode:bitmask <bitmask_or_code>
```

ตัวอย่างการใช้งานจริง:

ตัวอย่างที่ 1: สร้างระบบจัดการสินค้าสำหรับผู้ขาย

```bash
php artisan generate:module 31.01.04.262143 --role=vendor --verbose
```

· 31 = VendorProduct Module
· 01 = Controller Layer
· 04 = Import/Export Behavior
· 262143 = เปิดฟีเจอร์ทั้งหมด 18 ตัวแรก
· --role=vendor = สำหรับผู้ขาย
· --verbose = แสดงรายละเอียด

ผลลัพธ์:

```
✅ Generated 24 files for VendorProduct module
📁 Total: 3,600 lines of code
⏱️  Time: 12 seconds
```

ตัวอย่างที่ 2: สร้างระบบสั่งซื้อสำหรับลูกค้า

```bash
php artisan generate:module 23.01.07.65535 --role=customer
```

ตัวอย่างที่ 3: สร้างแดชบอร์ดสำหรับแอดมิน

```bash
php artisan generate:module 44.01.03.2097151 --role=admin
```

🔢 1.3 ตารางรหัสโมดูล (Module Codes Reference)

ID Code ชื่อโมดูล บทบาท รหัสเริ่มต้น
1 AUTH Authentication Core 01.01.01.255
2 USER User Management Core 02.01.01.127
3 PROF Profile Customer 03.01.01.63
18 PROD Product Public 18.01.04.4095
23 ORD Order Customer 23.01.07.65535
31 VEN_PROD Vendor Product Vendor 31.01.04.262143
35 VEN_ORD Vendor Order Vendor 35.01.07.16383
44 ADM_DASH Admin Dashboard Admin 44.01.03.2097151

⚙️ 1.4 ตารางฟีเจอร์ (Features Reference)

Bit ฐาน16 ฐาน10 ชื่อ คำอธิบาย
0 0x000001 1 LOGGING ระบบบันทึกการทำงาน
1 0x000002 2 CACHING ระบบแคช
2 0x000004 4 NOTIFICATIONS การแจ้งเตือน
3 0x000008 8 VALIDATION การตรวจสอบข้อมูล
4 0x000010 16 AUTHENTICATION การยืนยันตัวตน
5 0x000020 32 AUTHORIZATION การกำหนดสิทธิ์
6 0x000040 64 AUDITING การตรวจสอบการเปลี่ยนแปลง
7 0x000080 128 RATE_LIMITING จำกัดการเรียกใช้
8 0x000100 256 INVENTORY_CHECK ตรวจสอบสต็อก
9 0x000200 512 PRICE_VALIDATION ตรวจสอบราคา
10 0x000400 1024 TAX_CALCULATION คำนวณภาษี
11 0x000800 2048 DISCOUNT_APPLICATION ส่วนลด
12 0x001000 4096 SHIPPING_CALCULATION คำนวณค่าจัดส่ง
13 0x002000 8192 COMMISSION_CALCULATION คำนวณค่าคอมมิชชั่น
14 0x004000 16384 REFUND_PROCESSING การคืนเงิน
15 0x008000 32768 RETURN_VALIDATION การคืนสินค้า

🧮 1.5 วิธีคำนวณรหัสฟีเจอร์ (Feature Calculation)

วิธีที่ 1: บวกค่าฐาน 10

```php
// ต้องการ: Logging + Caching + Notifications + Validation
// ค่าฐาน 10: 1 + 2 + 4 + 8 = 15
php artisan generate:module 31.01.04.15
```

วิธีที่ 2: บวกค่าฐาน 16

```php
// ต้องการ: Inventory + Price + Tax + Shipping
// ค่าฐาน 16: 0x000100 + 0x000200 + 0x000400 + 0x001000 = 0x001700 (5888)
php artisan generate:module 31.01.04.5888
```

วิธีที่ 3: ใช้ค่าที่กำหนดไว้แล้ว

```php
// ค่าที่ใช้บ่อย
255     = ฟีเจอร์พื้นฐาน 8 ตัวแรก (0-7)
4095    = ฟีเจอร์พื้นฐาน + ฟีเจอร์ธุรกิจ 4 ตัวแรก (0-11)
65535   = ฟีเจอร์พื้นฐาน + ฟีเจอร์ธุรกิจทั้งหมด (0-15)
262143  = ฟีเจอร์ทั้งหมด 18 ตัวแรก
```

🖥️ 1.6 ตัวอย่างครบวงจร (Complete Example)

สร้างระบบขายของออนไลน์เต็มรูปแบบ:

```bash
#!/bin/bash
# generate-full-ecommerce.sh

echo "🚀 Starting E-commerce Platform Generation..."
echo ""

# Phase 1: Core Modules
echo "📦 Phase 1: Core Modules (13 modules)"
php artisan generate:module 01.01.01.255 --role=admin
php artisan generate:module 02.01.01.127 --role=admin
php artisan generate:module 03.01.01.63 --role=customer
php artisan generate:module 04.01.01.31 --role=customer
php artisan generate:module 05.01.07.15 --role=admin
php artisan generate:module 06.01.04.7 --role=vendor
php artisan generate:module 07.01.03.3 --role=public
php artisan generate:module 08.01.11.4095 --role=vendor
php artisan generate:module 09.01.12.2047 --role=admin
php artisan generate:module 10.01.08.1023 --role=admin
php artisan generate:module 11.01.01.511 --role=public
php artisan generate:module 12.01.06.255 --role=admin
php artisan generate:module 13.01.05.127 --role=admin

# Phase 2: Frontend
echo ""
echo "🛍️ Phase 2: Frontend Modules (9 modules)"
php artisan generate:module 14.01.01.63 --role=public
php artisan generate:module 15.01.03.31 --role=public
php artisan generate:module 16.01.01.15 --role=admin
php artisan generate:module 17.01.01.7 --role=admin
php artisan generate:module 18.01.04.4095 --role=vendor
php artisan generate:module 19.01.01.2047 --role=customer
php artisan generate:module 20.01.07.65535 --role=customer
php artisan generate:module 21.01.01.32767 --role=public
php artisan generate:module 22.01.01.16383 --role=admin

# Phase 3: Vendor Modules
echo ""
echo "🏪 Phase 3: Vendor Modules (15 modules)"
for i in {28..42}; do
    php artisan generate:module $(printf "%02d" $i).01.01.4095 --role=vendor
done

echo ""
echo "✅ E-commerce Platform Generated Successfully!"
```

🔧 ส่วนที่ 2: การปรับปรุงและพัฒนาระบบ (Development Guide)

🏗️ 2.1 โครงสร้างโปรเจค (Project Structure)

```
ecommerce-generator/
├── app/
│   ├── Services/CodeGenerator/
│   │   ├── Core/              # ระบบหลัก
│   │   │   ├── BitmaskDecoder.php      # ถอดรหัส bitmask
│   │   │   ├── LayerWarpEngine.php     # สร้างหลาย layer
│   │   │   ├── TemplateRenderer.php    # render template
│   │   │   ├── FileWriter.php          # เขียนไฟล์
│   │   │   └── DependencyResolver.php  # จัดการ dependencies
│   │   ├── BusinessLogic/     # โลจิกธุรกิจ
│   │   │   ├── BusinessDecisionTree.php
│   │   │   ├── ComplexLogicInjector.php
│   │   │   ├── EcommerceLogicLibrary.php
│   │   │   └── RoleBasedLogicGenerator.php
│   │   └── Optimizers/        # ปรับปรุงประสิทธิภาพ
│   │       ├── CodeOptimizer.php
│   │       ├── CacheOptimizer.php
│   │       └── QueryOptimizer.php
│   └── Console/Commands/      # คำสั่ง CLI
│       ├── GenerateModuleCommand.php
│       ├── GenerateAllModulesCommand.php
│       └── DecodeBitmaskCommand.php
├── config/
│   ├── generator.php          # การตั้งค่าหลัก
│   ├── dictionary.php         # พจนานุกรมกลาง
│   └── modules.php            # ข้อมูลโมดูล
├── resources/templates/       # ไฟล์ template
│   ├── layers/               # Template แต่ละ layer
│   └── business_logic/       # โลจิกธุรกิจ
└── Modules/                  # ผลลัพธ์ที่ generate
    ├── AUTH/                 # โมดูลที่ 1
    ├── USER/                 # โมดูลที่ 2
    └── ...                   # โมดูลอื่นๆ
```

📝 2.2 การเพิ่มโมดูลใหม่ (Adding New Module)

ขั้นตอนที่ 1: เพิ่มข้อมูลโมดูลใน dictionary

```php
// config/dictionary/modules.php
return [
    // ... โมดูลที่มีอยู่ ...
    
    '53' => [  // หมายเลขโมดูลใหม่
        'code' => 'NEW_MOD',
        'name' => 'NewModule',
        'full_name' => 'New E-commerce Module',
        'description' => 'Description of the new module',
        'role_access' => ['admin', 'vendor'],
        'dependencies' => [1, 2, 3], // ขึ้นกับโมดูลไหนบ้าง
        'business_logic_tags' => ['new_feature', 'custom_logic'],
        'bitmask_default' => '53.01.01.31' // รหัสเริ่มต้น
    ],
    
    '54' => [
        // ... โมดูลเพิ่มเติม ...
    ]
];
```

ขั้นตอนที่ 2: สร้าง template สำหรับโมดูลใหม่

```bash
# สร้าง directory สำหรับ template
mkdir -p resources/templates/layers/01_controller/53_new_module/
mkdir -p resources/templates/layers/02_service/53_new_module/
# ... สร้างสำหรับทุก layer ...

# สร้าง template ไฟล์
touch resources/templates/layers/01_controller/53_new_module/01_crud.stub
touch resources/templates/layers/01_controller/53_new_module/02_api.stub
# ... สร้าง template ทุก behavior ...
```

ขั้นตอนที่ 3: เพิ่ม business logic

```php
// app/Services/CodeGenerator/BusinessLogic/EcommerceLogicLibrary.php
class EcommerceLogicLibrary
{
    public function getNewModuleLogic(array $decoded, string $role): array
    {
        return [
            'service_methods' => [
                'processNewModule' => $this->generateNewModuleMethod(),
                'validateNewModule' => $this->generateValidationLogic(),
                // ... methods อื่นๆ ...
            ],
            'validation_rules' => [
                'field1' => 'required|string|max:255',
                'field2' => 'required|numeric|min:0',
                // ... rules อื่นๆ ...
            ],
            'business_rules' => [
                'rule1' => 'if condition then action',
                'rule2' => 'business logic here',
                // ... rules อื่นๆ ...
            ]
        ];
    }
    
    private function generateNewModuleMethod(): string
    {
        return <<<'METHOD'
    public function processNewModule(array $data): array
    {
        return DB::transaction(function () use ($data) {
            // Custom business logic here
            $processed = $this->customLogic($data);
            
            // Save to database
            $record = $this->repository->create($processed);
            
            // Trigger events
            event(new NewModuleProcessed($record));
            
            return [
                'success' => true,
                'data' => $record,
                'message' => 'New module processed successfully'
            ];
        });
    }
METHOD;
    }
}
```

ขั้นตอนที่ 4: อัพเดท dependencies

```php
// config/modules.php
return [
    'dependencies' => [
        // ... dependencies เดิม ...
        '53' => [1, 18, 31], // NewModule ขึ้นกับ Auth, Product, VendorProduct
        '23' => [19, 20, 24, 25, 53], // Order ขึ้นกับ NewModule ด้วย
    ],
    
    'generation_order' => [
        // ... phases เดิม ...
        'phase_9' => [53, 54, 55] // เพิ่ม phase ใหม่สำหรับโมดูลใหม่
    ]
];
```

ขั้นตอนที่ 5: Generate โมดูลใหม่

```bash
# ทดสอบ generate โมดูลใหม่
php artisan generate:module 53.01.01.31 --role=admin --dry-run --verbose

# Generate จริง
php artisan generate:module 53.01.01.31 --role=admin --force
```

🎨 2.3 การเพิ่ม Layer ใหม่ (Adding New Layer)

ขั้นตอนที่ 1: ลงทะเบียน Layer ใหม่

```php
// config/dictionary/layers.php
return [
    // ... layers เดิม (1-24) ...
    
    '25' => [  // Layer ใหม่
        'code' => 'NEW_LAYER',
        'name' => 'New Layer',
        'full_name' => 'Custom Business Layer',
        'description' => 'New layer for custom business logic',
        'directory' => 'CustomLayers',
        'file_suffix' => 'CustomLayer',
        'base_class' => 'App\\Custom\\BaseCustomLayer',
        'required_methods' => ['process', 'validate', 'execute'],
        'template_type' => 'custom_layer',
        'dependency_pattern' => 'Service + Repository'
    ]
];
```

ขั้นตอนที่ 2: สร้าง Template

```bash
# สร้าง template directory
mkdir -p resources/templates/layers/25_new_layer/

# สร้าง template สำหรับแต่ละ behavior
for i in {1..15}; do
    touch resources/templates/layers/25_new_layer/$(printf "%02d" $i)_behavior.stub
done

# สร้าง template หลัก
cat > resources/templates/layers/25_new_layer/default.stub << 'EOF'
<?php
/**
 * {{MODULE_FULL_NAME}} - {{LAYER_NAME}}
 */

namespace {{NAMESPACE}};

use {{MODULE_NAMESPACE}}\Services\{{MODULE_NAME}}Service;
use {{MODULE_NAMESPACE}}\Repositories\{{MODULE_NAME}}RepositoryInterface;

class {{CLASS_NAME}}
{
    protected $service;
    protected $repository;
    
    public function __construct(
        {{MODULE_NAME}}Service $service,
        {{MODULE_NAME}}RepositoryInterface $repository
    ) {
        $this->service = $service;
        $this->repository = $repository;
    }
    
    {{METHODS}}
    
    {{CUSTOM_BUSINESS_LOGIC}}
}
EOF
```

ขั้นตอนที่ 3: ปรับปรุง Layer Warp Engine

```php
// app/Services/CodeGenerator/Core/LayerWarpEngine.php
class LayerWarpEngine
{
    private function generateAllLayers(array $decoded, array $moduleConfig, string $role): array
    {
        $layers = [];
        $allLayers = config('dictionary.layers');
        
        foreach ($allLayers as $layerId => $layerConfig) {
            // เพิ่ม logic สำหรับ layer ใหม่
            if ($layerId == 25) { // New Layer
                $layer = $this->generateCustomLayer($layerId, $decoded, $moduleConfig, $role);
            } else {
                $layer = $this->generateLayer($layerId, $decoded, $moduleConfig, $role);
            }
            
            if ($layer) {
                $layers[$layerId] = $layer;
            }
        }
        
        return $layers;
    }
    
    private function generateCustomLayer(int $layerId, array $decoded, array $moduleConfig, string $role): array
    {
        // Custom generation logic สำหรับ layer ใหม่
        $layerConfig = config("dictionary.layers.{$layerId}");
        
        return [
            'id' => $layerId,
            'name' => $layerConfig['name'],
            'directory' => $layerConfig['directory'],
            'file_suffix' => $layerConfig['file_suffix'],
            'content' => $this->generateCustomContent($layerId, $decoded, $moduleConfig, $role),
            // ... fields อื่นๆ ...
        ];
    }
}
```

🔌 2.4 การเพิ่ม Behavior ใหม่ (Adding New Behavior)

ขั้นตอนที่ 1: ลงทะเบียน Behavior ใหม่

```php
// config/dictionary/behaviors.php
return [
    // ... behaviors เดิม (1-15) ...
    
    '16' => [  // Behavior ใหม่
        'code' => 'CUSTOM_BEHAV',
        'name' => 'Custom Behavior',
        'description' => 'Custom business behavior with special logic',
        'applicable_layers' => ['controller', 'service', 'repository'],
        'methods_generated' => ['customMethod1', 'customMethod2', 'customProcess'],
        'routes_generated' => [
            'GET' => ['customIndex', 'customShow'],
            'POST' => ['customStore'],
            'PUT' => ['customUpdate'],
            'DELETE' => ['customDestroy']
        ],
        'validation_required' => true,
        'authorization_required' => true,
        'custom_logic' => true
    ]
];
```

ขั้นตอนที่ 2: สร้าง Template สำหรับทุก Layer

```bash
# สร้าง template สำหรับ behavior ใหม่ในทุก layer
for layer in {01..24}; do
    mkdir -p resources/templates/layers/${layer}_*/16_custom_behav.stub
done

# ตัวอย่าง template สำหรับ controller
cat > resources/templates/layers/01_controller/16_custom_behav.stub << 'EOF'
<?php
namespace {{NAMESPACE}};

use Illuminate\Http\Request;
use App\Http\Controllers\Controller;

class {{CLASS_NAME}} extends Controller
{
    public function customIndex(Request $request)
    {
        {{CUSTOM_LOGIC}}
        
        return view('{{MODULE_CODE}}.custom.index', [
            'data' => $data
        ]);
    }
    
    public function customStore(Request $request)
    {
        {{CUSTOM_VALIDATION}}
        
        $result = $this->service->customProcess($validated);
        
        {{CUSTOM_NOTIFICATION}}
        
        return redirect()->route('{{MODULE_CODE}}.custom.index')
            ->with('success', 'Custom process completed');
    }
    
    {{CUSTOM_METHODS}}
}
EOF
```

ขั้นตอนที่ 3: เพิ่ม Business Logic

```php
// app/Services/CodeGenerator/BusinessLogic/CustomBehaviorLogic.php
class CustomBehaviorLogic
{
    public function injectCustomLogic(array $layer, array $decoded): array
    {
        if ($decoded['behavior'] == 16) {
            $customLogic = $this->generateCustomLogic($decoded);
            $layer['content'] = str_replace(
                '{{CUSTOM_LOGIC}}',
                $customLogic,
                $layer['content']
            );
        }
        
        return $layer;
    }
    
    private function generateCustomLogic(array $decoded): string
    {
        return <<<'LOGIC'
        // Custom business logic
        $data = $this->service->getCustomData([
            'filters' => $request->input('filters', []),
            'sort_by' => $request->input('sort_by', 'created_at'),
            'sort_dir' => $request->input('sort_dir', 'desc')
        ]);
        
        // Apply custom transformations
        $data = $this->applyCustomTransformations($data);
        
        // Cache for performance
        $cacheKey = 'custom_data_' . md5(serialize($request->all()));
        $data = Cache::remember($cacheKey, 300, function () use ($data) {
            return $data;
        });
LOGIC;
    }
}
```

⚡ 2.5 การเพิ่ม Feature ใหม่ (Adding New Feature)

ขั้นตอนที่ 1: เพิ่ม Feature ใน Bitmask System

```php
// config/generator.php
return [
    'bitmask' => [
        'sections' => [
            'features' => [
                'bits' => 18, // เพิ่ม bits ถ้าจำเป็น
                // ...
            ]
        ]
    ]
];

// config/dictionary/features.php
return [
    // ... features เดิม ...
    
    '0x100000' => [  // Bit 24
        'decimal' => 1048576,
        'name' => 'AI_RECOMMENDATION',
        'display_name' => 'AI Recommendation',
        'description' => 'AI-powered product recommendations',
        'layers_affected' => ['controller', 'service', 'repository'],
        'code_injection' => $this->getAiRecommendationCode(),
        'performance_impact' => 'medium',
        'dependencies' => ['MachineLearningService']
    ],
    
    '0x200000' => [  // Bit 25
        'decimal' => 2097152,
        'name' => 'REAL_TIME_ANALYTICS',
        'display_name' => 'Real-time Analytics',
        'description' => 'Real-time data analytics and dashboards',
        'layers_affected' => ['service', 'controller', 'job'],
        'code_injection' => $this->getAnalyticsCode(),
        'performance_impact' => 'high',
        'dependencies' => ['AnalyticsService', 'Redis']
    ]
];
```

ขั้นตอนที่ 2: สร้าง Code Injection

```php
// ใน config/dictionary/features.php
private function getAiRecommendationCode(): string
{
    return <<<'CODE'
    /**
     * Get AI-powered recommendations
     */
    public function getRecommendations(int $customerId, int $limit = 10): array
    {
        $cacheKey = "recommendations:{$customerId}:{$limit}";
        
        return Cache::remember($cacheKey, 3600, function () use ($customerId, $limit) {
            // Analyze customer behavior
            $behavior = $this->analyzeCustomerBehavior($customerId);
            
            // Get similar customers
            $similarCustomers = $this->findSimilarCustomers($customerId);
            
            // Generate recommendations
            $recommendations = $this->mlService->generateRecommendations([
                'customer_id' => $customerId,
                'behavior' => $behavior,
                'similar_customers' => $similarCustomers,
                'limit' => $limit
            ]);
            
            // Filter available products
            $recommendations = $this->filterAvailableProducts($recommendations);
            
            return $recommendations;
        });
    }
    
    /**
     * Analyze customer behavior for recommendations
     */
    private function analyzeCustomerBehavior(int $customerId): array
    {
        $purchaseHistory = $this->repository->getPurchaseHistory($customerId);
        $browsingHistory = $this->repository->getBrowsingHistory($customerId);
        $wishlistItems = $this->repository->getWishlistItems($customerId);
        
        return [
            'purchase_patterns' => $this->extractPatterns($purchaseHistory),
            'browsing_interests' => $this->analyzeInterests($browsingHistory),
            'wishlist_preferences' => $this->analyzePreferences($wishlistItems),
            'customer_segment' => $this->determineSegment($customerId)
        ];
    }
CODE;
}
```

ขั้นตอนที่ 3: อัพเดท Feature Injector

```php
// app/Services/CodeGenerator/BusinessLogic/ComplexLogicInjector.php
class ComplexLogicInjector
{
    private function injectFeatureLogic(array $layer, int $featuresBitmask): array
    {
        $bitmaskDecoder = new BitmaskDecoder();
        $enabledFeatures = $bitmaskDecoder->extractFeatures($featuresBitmask);
        
        foreach ($enabledFeatures as $feature) {
            // เพิ่มเงื่อนไขสำหรับ feature ใหม่
            if ($feature['name'] === 'AI_RECOMMENDATION') {
                $layer = $this->injectAiRecommendation($layer);
            }
            
            if ($feature['name'] === 'REAL_TIME_ANALYTICS') {
                $layer = $this->injectRealTimeAnalytics($layer);
            }
            
            // ... features อื่นๆ ...
        }
        
        return $layer;
    }
    
    private function injectAiRecommendation(array $layer): array
    {
        if ($layer['id'] == 2) { // Service layer
            $aiCode = config('dictionary.features.0x100000.code_injection');
            $layer['content'] = str_replace(
                '{{AI_RECOMMENDATION_LOGIC}}',
                $aiCode,
                $layer['content']
            );
        }
        
        return $layer;
    }
}
```

🔄 2.6 การปรับปรุง Business Logic

ตัวอย่าง: เพิ่ม Complex Pricing Logic

```php
// app/Services/CodeGenerator/BusinessLogic/PricingLogic.php
class PricingLogic
{
    public function generateDynamicPricing(): string
    {
        return <<<'PRICING'
    /**
     * Calculate dynamic price with multiple factors
     */
    public function calculateDynamicPrice(Product $product, Customer $customer, array $context): array
    {
        $basePrice = $product->base_price;
        
        // 1. Vendor-specific pricing
        $vendorPrice = $this->applyVendorPricing($basePrice, $product->vendor_id);
        
        // 2. Customer tier pricing
        $tierPrice = $this->applyCustomerTier($vendorPrice, $customer->tier);
        
        // 3. Real-time competitor analysis
        $competitivePrice = $this->adjustForCompetition($tierPrice, $product->sku);
        
        // 4. Demand-based pricing
        $demandPrice = $this->adjustForDemand($competitivePrice, $product->sales_velocity);
        
        // 5. Time-based pricing
        $timePrice = $this->applyTimeBasedPricing($demandPrice);
        
        // 6. Bundle/multi-item discount
        $bundlePrice = $this->applyBundleDiscount($timePrice, $context['cart_items'] ?? []);
        
        // 7. Location-based pricing
        $locationPrice = $this->applyLocationPricing($bundlePrice, $context['shipping_address']);
        
        // 8. Personalization (customer's price sensitivity)
        $personalizedPrice = $this->personalizePrice($locationPrice, $customer->price_sensitivity);
        
        // 9. Minimum/maximum constraints
        $finalPrice = $this->applyPriceConstraints($personalizedPrice, [
            'min' => $product->cost_price * 1.2,
            'max' => $product->msrp
        ]);
        
        // 10. Psychological pricing ($19.99 not $20.00)
        $finalPrice = $this->applyPsychologicalPricing($finalPrice);
        
        return [
            'price' => $finalPrice,
            'breakdown' => [
                'base_price' => $basePrice,
                'vendor_markup' => $vendorPrice - $basePrice,
                'customer_discount' => $tierPrice - $vendorPrice,
                'competitor_adjustment' => $competitivePrice - $tierPrice,
                'demand_adjustment' => $demandPrice - $competitivePrice,
                'time_adjustment' => $timePrice - $demandPrice,
                'bundle_discount' => $bundlePrice - $timePrice,
                'location_adjustment' => $locationPrice - $bundlePrice,
                'personalization' => $personalizedPrice - $locationPrice,
                'final_adjustment' => $finalPrice - $personalizedPrice
            ],
            'factors_applied' => $this->getAppliedFactors()
        ];
    }
PRICING;
    }
}
```

🧪 2.7 การทดสอบระบบ (Testing)

Unit Tests:

```php
// tests/Unit/BitmaskTest.php
class BitmaskTest extends TestCase
{
    public function test_bitmask_decoding()
    {
        $decoder = new BitmaskDecoder();
        
        $result = $decoder->decode('31.01.04.262143');
        
        $this->assertEquals(31, $result['module']);
        $this->assertEquals(1, $result['layer']);
        $this->assertEquals(4, $result['behavior']);
        $this->assertEquals(262143, $result['features']);
    }
    
    public function test_feature_extraction()
    {
        $decoder = new BitmaskDecoder();
        
        $features = $decoder->extractFeatures(262143); // 0x3FFFF
        
        $this->assertArrayHasKey('0x000001', $features); // LOGGING
        $this->assertArrayHasKey('0x000002', $features); // CACHING
        $this->assertArrayHasKey('0x008000', $features); // RETURN_VALIDATION
    }
    
    public function test_code_generation()
    {
        $generator = new LayerWarpEngine();
        
        $result = $generator->warp('31.01.04.31', 'vendor');
        
        $this->assertArrayHasKey('layers', $result);
        $this->assertArrayHasKey('files', $result);
        $this->assertGreaterThan(0, count($result['files']));
    }
}
```

Integration Tests:

```php
// tests/Feature/GenerateModuleTest.php
class GenerateModuleTest extends TestCase
{
    public function test_generate_vendor_product_module()
    {
        $this->artisan('generate:module', [
            'code' => '31.01.04.31',
            '--dry-run' => true,
            '--verbose' => true
        ])->assertExitCode(0);
    }
    
    public function test_generate_all_modules_dry_run()
    {
        $this->artisan('generate:all', [
            '--dry-run' => true,
            '--phase' => '1'
        ])->assertExitCode(0);
    }
    
    public function test_file_generation()
    {
        $fileWriter = new FileWriter();
        $files = [
            [
                'path' => storage_path('test/TestController.php'),
                'content' => '<?php // Test content'
            ]
        ];
        
        $result = $fileWriter->writeBatch($files, true);
        
        $this->assertTrue($result['success'] > 0);
        $this->assertFileExists(storage_path('test/TestController.php'));
        
        // Cleanup
        unlink(storage_path('test/TestController.php'));
        rmdir(storage_path('test'));
    }
}
```

Performance Tests:

```php
// tests/Performance/GenerationPerformanceTest.php
class GenerationPerformanceTest extends TestCase
{
    public function test_generation_speed()
    {
        $start = microtime(true);
        
        $generator = new LayerWarpEngine();
        
        for ($i = 0; $i < 10; $i++) {
            $generator->warp('31.01.04.31', 'vendor');
        }
        
        $end = microtime(true);
        $time = $end - $start;
        
        $this->assertLessThan(5.0, $time, 
            "Generation should take less than 5 seconds for 10 modules"
        );
    }
    
    public function test_memory_usage()
    {
        $memoryBefore = memory_get_usage();
        
        $generator = new LayerWarpEngine();
        $result = $generator->warp('31.01.04.262143', 'vendor');
        
        $memoryAfter = memory_get_usage();
        $memoryUsed = $memoryAfter - $memoryBefore;
        
        $this->assertLessThan(50 * 1024 * 1024, $memoryUsed, 
            "Memory usage should be less than 50MB"
        );
    }
}
```

📊 2.8 การตรวจสอบและแก้ปัญหา (Troubleshooting)

ปัญหาทั่วไปและวิธีแก้:

ปัญหา 1: Bitmask ไม่ถูกต้อง

```
Error: Invalid code format. Expected: module.layer.behavior.features
```

แก้ไข: ตรวจสอบรูปแบบ xx.xx.xx.xxxx และค่าที่อยู่ภายใน range

```bash
# ตรวจสอบ range
Module: 1-52
Layer: 1-24
Behavior: 1-15
Features: 0-262143
```

ปัญหา 2: Template ไม่พบ

```
Error: Template not found for layer X, behavior Y
```

แก้ไข: สร้าง template ที่ขาดหายไป

```bash
# สร้าง template ที่ขาด
touch resources/templates/layers/$(printf "%02d" $layer)_*/$(printf "%02d" $behavior)_*.stub
```

ปัญหา 3: Memory limit เกิน

```
Error: Allowed memory size exhausted
```

แก้ไข: เพิ่ม memory limit หรือ optimize code

```php
// ใน config/generator.php
'performance' => [
    'memory' => [
        'memory_limit' => '512M' // เพิ่มจาก 256M
    ]
]
```

ปัญหา 4: Generation ช้า
แก้ไข:เปิด caching และ optimize

```bash
# เปิด template caching
php artisan config:cache
php artisan route:cache

# ใช้ parallel generation (ถ้า support)
php artisan generate:all --parallel
```

🔍 2.9 Debugging Tools

Debug Command:

```bash
# แสดงข้อมูล bitmask อย่างละเอียด
php artisan decode:bitmask 31.01.04.262143 --verbose

# ทดสอบ generate โดยไม่เขียนไฟล์
php artisan generate:module 31.01.04.31 --dry-run --verbose

# Profile memory usage
php artisan generate:module 31.01.04.31 --profile-memory

# Generate และดู statistics
php artisan generate:all --report --output=storage/reports/
```

Log File:

```php
// เปิด debug logging
// ใน config/generator.php
'debug' => [
    'log_generation' => true,
    'log_path' => storage_path('logs/code-generator.log'),
    'log_level' => 'debug'
]
```

📈 2.10 Best Practices

1. ใช้ Version Control

```bash
# ก่อน generate ใหม่
git add Modules/
git commit -m "Generated modules before changes"

# หลัง generate
git diff Modules/
git add Modules/
git commit -m "Regenerated modules with new features"
```

2. Backup ก่อน Generate

```bash
# Backup โมดูลเดิม
cp -r Modules/ Modules_backup_$(date +%Y%m%d_%H%M%S)/

# หรือใช้ --skip-existing
php artisan generate:all --skip-existing
```

3. Test ก่อน Production

```bash
# Test ใน development
php artisan generate:module 31.01.04.31 --dry-run
php artisan test

# Test ใน staging
php artisan generate:all --phase=1 --dry-run
```

4. Document การเปลี่ยนแปลง

```markdown
# เอกสารการเปลี่ยนแปลง

## วันที่: 2024-01-15
### การเปลี่ยนแปลง:
1. เพิ่มโมดูลใหม่: AI Recommendations (ID: 53)
2. เพิ่ม feature: Real-time Analytics (Bit: 25)
3. ปรับปรุง pricing logic

### รหัสที่ใช้:
- AI Recommendations: 53.01.16.1048576
- Vendor Product with AI: 31.01.04.3670015 (262143 + 1048576)

### ผลกระทบ:
- เพิ่มขนาดโค้ดประมาณ 5,000 บรรทัด
- เพิ่ม dependencies: machine_learning_service
```

🚀 ส่วนที่ 3: การใช้งานขั้นสูง (Advanced Usage)

3.1 Custom Template System

```php
// สร้าง custom template engine
class CustomTemplateRenderer extends TemplateRenderer
{
    public function renderWithCustomLogic(string $templatePath, array $variables): string
    {
        // Custom template processing
        $content = parent::render($templatePath, $variables);
        
        // Add custom annotations
        $content = $this->addCustomAnnotations($content, $variables);
        
        // Apply custom optimizations
        $content = $this->applyCustomOptimizations($content);
        
        return $content;
    }
}
```

3.2 Plugin System

```php
// สร้าง plugin สำหรับ feature ใหม่
class AnalyticsPlugin implements GeneratorPlugin
{
    public function beforeGenerate(array $config): array
    {
        // Modify config before generation
        if ($config['features'] & 0x200000) { // REAL_TIME_ANALYTICS
            $config['dependencies'][] = 'analytics-service';
        }
        
        return $config;
    }
    
    public function afterGenerate(array $result): array
    {
        // Post-generation processing
        if (isset($result['metadata']['features']['REAL_TIME_ANALYTICS'])) {
            $this->setupAnalytics($result);
        }
        
        return $result;
    }
}
```

3.3 Multi-environment Configuration

```php
// config/generator.staging.php
return [
    'generation' => [
        'output_path' => base_path('staging/Modules'),
        'namespace_prefix' => 'Staging\\Modules',
        'auto_generate' => [
            'tests' => true,
            'documentation' => false
        ]
    ]
];

// config/generator.production.php
return [
    'generation' => [
        'output_path' => base_path('production/Modules'),
        'optimizations' => [
            'minify_code' => true,
            'remove_comments' => true
        ]
    ]
];
```

📚 สรุป

Key Points:

1. ใช้ Bitmask System สำหรับการกำหนดค่าที่กระชับ
2. Layer Warp Engine สร้างหลาย layer พร้อมกัน
3. Business Logic Injection เพิ่มโลจิกที่ซับซ้อนอัตโนมัติ
4. Dictionary-based ระบบกลางสำหรับการจัดการข้อมูล
5. Extensible สามารถเพิ่มโมดูล, layer, behavior, feature ใหม่ได้

Workflow การพัฒนา:

```
1. กำหนด requirement → 2. ออกแบบ bitmask → 3. สร้าง template
     ↓                                    ↓
4. เพิ่ม business logic ← 5. ทดสอบ ← 6. Generate
     ↓
7. Deploy → 8. Monitor → 9. Iterate
```

ติดต่อและสนับสนุน:

· Documentation: /docs directory
· Issues: GitHub Issues
· Contributing: CONTRIBUTING.md
· Support: dev-35@runfuture.io

